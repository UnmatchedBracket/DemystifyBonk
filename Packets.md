# Packets
If you've connected to a bonk.io server, these are the packets to use.

Each packet usually starts with "42" and then has a JSON list. The first item in the list specifies what type of packet it is.

Packets that break this rule:
<ul>
  <li><code>3probe</code>: <code>2probe</code>'s response</li>
  <li><code>3</code>: <code>2</code>'s response (a heartbeat)</li>
  <li><code>40</code>: Socket established?? (in response to <code>5</code>)</li>
  <li><code>41</code>: WebSocket closed by server. This usually means you've been kicked from the lobby.</li>
</ul>

<span id="common_schemes"></span>
## Common Schemes
### Team
|0|1|2|3|4|5|
|-|-|-|-|-|-|
|Spec|FFA|Red|Blue|Green|Yellow|

## Contents
### Incoming:
<ul>
<li><a href="#inc1">1: Update Pings</a></li>
<li><a href="#inc3">3: Room join</a></li>
<li><a href="#inc4">4: Player join</a></li>
<li><a href="#inc5">5: Player leave</a></li>
<li><a href="#inc6">6: Host leave</a></li>
<li><a href="#inc7">7: Inputs</a></li>
<li><a href="#inc8">8: Ready Change</a></li>
<li><a href="#inc13">13: Game End</a></li>
<li><a href="#inc15">15: Game Start</a></li>
<li><a href="#inc16">16: Error</a></li>
<li><a href="#inc18">18: Team Change</a></li>
<li><a href="#inc19">19: Teamlock toggle</a></li>
<li><a href="#inc20">20: Chat Message</a></li>
<li><a href="#inc21">21: Initial data</a></li>
<li><a href="#inc26">26: Mode change</a></li>
<li><a href="#inc27">27: Change WL (Rounds)</a></li>
<li><a href="#inc29">29: Map switch</a></li>
<li><a href="#inc33">33: Map Suggest</a></li>
<li><a href="#inc34">34: Map Suggest Client</a></li>
<li><a href="#inc40">40: Save Replay</a></li>
<li><a href="#inc40">43: Game starting Countdown</a></li>
<li><a href="#inc44">44: Abort Countdown</a></li>
<li><a href="#inc46">46: Local Gained XP</a></li>
<li><a href="#inc52">52: Tabbed</a></li>
<li><a href="#inc58">58: Room Name Update</a></li>
<li><a href="#inc59">59: Room Password Update</a></li> 
</ul>

### Outgoing
<ul>
<li><a href="#out3">3: Send Inputs</a></li> 
<li><a href="#out5">5: Trigger Start</a></li> 
<li><a href="#out6">6: Change Own Team</a></li> 
<li><a href="#out7">7: Team Lock</a></li> 
<li><a href="#out9">9: Ban Player</a></li> 
<li><a href="#out10">10: Chat Message</a></li>
<li><a href="#out11">11: inform In Lobby</a></li> 
<li><a href="#out12">12: create Room</a></li> 
<li><a href="#out14">14: Return To Lobby</a></li> 
<li><a href="#out16">16: Set Ready</a></li> 
<li><a href="#out20">20: send GAMO</a></li> 
<li><a href="#out21">21: send WL (Rounds)</a></li> 
<li><a href="#out23">23: send Map Add</a></li> 
<li><a href="#out26">26: change Other Team</a></li> 
<li><a href="#out27">27: send Map Suggest</a></li> 
<li><a href="#out29">29: send Balance</a></li> 
<li><a href="#out32">32: send Team Settings Change</a></li> 
<li><a href="#out33">33: send Arm Record</a></li> 
</ul>

_____


## Incoming

<ul>
  <li id="inc1"><p>
    1: Update Pings
    <br>This packet is used to provide info about other player's pings, as well as to measure your own.
    <br>Example: <code>42[1,{"30":180,"33":148,"34":190},9]</code>
    <br>Items:
    <ol type=1>
      <li>An object with the other user's ping times.</li>
      <li>An number to determine which ping a client is responding to. The client should echo <code>42[1,{"id":<this number>}]</code> when this packet is recieved.</li>
    </ol>
  </p></li>
  <li id="inc3"><p>
    3: Room join
    <br>This packet is used to provide some information about a lobby when you join it.
    <br>Example: <code>42[3,3,0,[{"peerID":"vuzvugdrnja00000","userName":"user one","guest":true,"team":1,"level":0,"ready":false,"tabbed":true,"avatar":{"layers":[],"bc":9315498}},null,null,{"peerID":"nx25am3w8d700000","userName":"left paren","guest":false,"team":1,"level":106,"ready":false,"tabbed":false,"avatar":{"layers":[{"id":34,"scale":0.4000000059604645,"angle":-90,"x":-5,"y":0,"flipX":false,"flipY":false,"color":1641226}],"bc":61591}}],0,false,901003,"mtomw",null]</code>
    <br>Items:
    <ol type=1>
      <li>Your ID.</li>
      <li>Host's ID.</li>
      <li>A list of players, starting at ID 0 and increasing by 1 each item. <code>null</code> indicates that player left. Each player still present is represented by an object:
      <ul>
        <li>"peerId": Peer ID. Useless for most purposes.</li>
        <li>"userName": The player's username.</li>
        <li>"guest": <code>true</code> if the player is a guest, otherwise <code>false</code>.</li>
        <li>"team": The player's team. See <a href="#common_schemes">Common Scemes</a></li>
        <li>"level": The player's level. 0 if the player is a guest.</li>
        <li>"ready": <code>true</code> if the player is ready (has a checkmark), otherwise <code>false</code>.</li>
        <li>"tabbed": <code>true</code> if the player is not focused on the tab, otherwise <code>false</code>.</li>
        <li>"avatar": A JSON object representing the player's skin.</li>
      </ul>
      </li>
      <li>Server Unix timestamp??</li>
      <li><code>true</code> if teams are locked, otherwise <code>false</code>.</li>
      <li>Room ID.</li>
      <li>Room bypass. Use with the Room ID to get the share link (https://bonk.io/&lt;room id right-padded to six digits&gt;&lt;room bypass&gt;)</li>
      <li>I have no clue what this is. AFAIK it's always <code>null</code></li>
    </ol>
  </p></li>
  <li id="inc4"><p>
    4: Player join
    <br>Example: <code>42[4,21,"0qh12mq737fh0000","left paren",false,39,1,{"layers":[{"id":34,"scale":0.4000000059604645,"angle":-90,"x":-5,"y":0,"flipX":false,"flipY":false,"color":1641226}],"bc":61591}]</code>
    <br>Items:
    <ol type=1>
      <li>The new player's ID.</li>
      <li>Peer ID. Useless for most purposes.</li>
      <li>The new player's username.</li>
      <li><code>true</code> if the new user is a guest, otherwise <code>false</code>.</li>
      <li>The new player's level.</li>
      <li>If the player is tabbed out?</li>
      <li>A JSON object representing the new player's skin.</li>
    </ol>
  </p></li>
  <li id="inc5"><p>
    5: Player leave
    <br>Example: <code>42[5,13,14511]</code>
    <br>Items:
    <ol type=1>
      <li>The player's ID.</li>
      <li>The tick on which they left?</li>
    </ol>
  </p></li>
  <li id="inc6"><p>
    6: Host leave
    <br>The host left, and may have closed the room.
    <br>Examples:
    <ul>
      <li><code>42[6,1,0,49753339064]</code></li>
      <li><code>42[6,0,-1,49753339073]</code></li>
    </ul>
    <br>Items:
    <ol type=1>
      <li>The old host's ID.</li>
      <li>The new host's ID. If this is -1, host closed the room.</li>
    </ol>
  </p></li>
  <li id="inc7"><p>
    7: Inputs
    <br>Examples:
    <ul>
      <li><code>42[7,1,{"i":38,"f":324,"c":45}]</code></li>
      <li><code>42[7,1,{"i":25,"f":531,"c":108}]</code></li>
    </ul>
    <br>Items:
    <ol type=1>
      <li>The ID. of the person that changed directions.</li>
      <li>An object:
      <ul>
        <li>"i": The inputs. Each bit is a direction. Left=1, right=2, up=4, down=8, heavy=16, special=32</li>
        <li>"f": The frame this input was preformed on.</li>
        <li>"c": Sequence number. Starts at 0 and lasts the entire session, increasing by 1 every time the player makes an input.</li>
      </ul>
      </li>
    </ol>
  </p></li>
  <li id="inc8"><p>
    8: Ready Change
    <br>Example: <code>42[8,1,true]</code>
    <br>Items:
    <ol type=1>
      <li>The ID of the person that changed their [READY] status</li>
      <li><code>true</code> if the player is ready (has a checkmark), otherwise <code>false</code>.</li>
    </ol>
  </p></li>
  <li id="inc13"><p>
    13: Game End
    <br>Example: <code>42[13]</code>
  </p></li>
  <li id="inc15"><p>
    15: Game Start
    <br>Example: <code>42[15,1658450696036,"jWCW9ahaqG6GsGbWmycybYaVyafa7GAqc0bXagWWe0agouIGdtGhSKWavaAGefSlIWqacsOcamIjdyjBcFqKukaGe4IUCdirGa1anYcAUrkFguaALYATYgFUoJ8xcFaALCYu4ARsQBiAVkL0+li42ChcAG64AMIAhjAAilwmcFSa3gAuERRRJvoIhFyi0IK0FDBUUaBFXGakOlB4AKJWWgAWHvFFUAB2aABWHgDM+qSsSAAiXFNJagDOqGgAnlwAZs6LoPQc3ivVZr59EJFRSIKNOVz63YTje4SzR9EeMIMo42vYJgizmoRUAJKCKCtJBmLQAVzArQw0AAjBCPGYMpo2BAuCg4ABLCwgVFQYhYuLxOxcQitCyMJhEklwHQMWjAFa4RpIbqtfJccZo4bODzhejc-TOCgrAVQQRCkj-C76PpIcKoriDKCwijhPaw8KM6Kg3AzdK4UHAACOmjQVDiAA9Ruw0d1ZVMALyOoA",{"map":"ILAMJAhBFBjBzCTlMiAJgNQEYFsCsAFtgOqYDWIADgM4BaJdoAkgBKSGy6YCuK-EAPTDhAd2QBhEOIGzgAXnlA","gt":2,"wl":3,"q":false,"tl":false,"tea":false,"ga":"b","mo":"b","bal":[]}]</code>
    <br>Items:
    <ol type=1>
      <li>Unix time of start??</li>
      <li>No clue <code>¯\_(ツ)_/¯</code></li>
      <li>An object with map + other game-related data.</li>
    </ol>
  </p></li>
  <li id="inc16"><p>
    16: Error
    <br>Example: <code>42[16,"rate_limit_ready"]</code>
    <br>Items:
    <ol type=1>
      <li>The error code. List (probably incomplete):
      <ul>
        <li>arm rate limited: You spammed save replay too fast</li>
        <li>room_full: You tried to join a full room.</li>
        <li>banned: You tried to join a room you've been kicked from.</li>
        <li>no_client_entry: You sent some action but you're not in a room??</li>
        <li>already_in_this_room: You tried to join a room that your user is already in.</li>
        <li>join_rate_limited: You've tried to join rooms too quickly.</li>
        <li>password_wrong: You tried to join a room, but you used the wrong password.</li>
        <li>guest: You attemped to preform an action that requires you to be logged in.</li>
        <li>rate_limit_ready: You hit the [READY] button too much.</li>
        <li>host_change_rate_limited: You tried to give host too much.</li>
        <li>rate_limit_mapsuggest: You suggested maps too quickly.</li>
        <li>rate_limit_countdown: You sent too many "Game starting in &lt;x&gt;" messages.</li>
        <li>rate_limit_abortcountdown: You sent too many "Countdown aborted!" messages.</li>
        <li>rate_limit_sma: You changed the map too quickly.</li>
        <li>rate_limit_cot: You changed teams too quickly.</li>
        <li>rate_limit_sgt: You changed modes too quickly</li>
        <li>rate_limit_tl: You locked the teams too quickly</li>
        <li>rate_limit: Generic rate-limit. You did something too fast.</li>
        <li>not_hosting: You attempted to do an action that requires you to be the game's host.</li>
        <li>cant_ban_yourself: You tried to kick yourself. You just can't do that.</li>
      </ul>
      </li>
    </ol>
  </p></li>
  <li id="inc18"><p>
    18: Team Change
    <br>Example: <code>42[18,2,0]</code>
    <br>Items:
    <ol type=1>
      <li>The ID of the person that changed teams</li>
      <li>The team they moved to (See <a href="#team_scheme">Common Scemes</a>)</li>
    </ol>
  </p></li>
  <li id="inc19"><p>
    19: Teamlock toggle
    <br>Example: <code>42[19,true]</code>
    <br>Items:
    <ol type=1>
      <li><code>true</code> if teams were locked, <code>false</code> if they were unlocked.</li>
    </ol>
  </p></li>
  <li id="inc20"><p>
    20: Chat Message
    <br>Example: <code>42[20,0,"hello"]</code>
    <br>Items:
    <ol type=1>
      <li>The ID of the person who sent the message.</li>
      <li>The chat message.</li>
    </ol>
  </p></li>
  <li id="inc21"><p>
    21: Initial data
    <br>Sent by the host after you join a room, this contains map and game data.
    <br>Example: <code>42[21,{"map":{"v":13,"s":{"re":false,"nc":false,"pq":1,"gd":25,"fl":false},"physics":{"shapes":[],"fixtures":[],"bodies":[],"bro":[],"joints":[],"ppm":12},"spawns":[],"capZones":[],"m":{"a":"user one","n":"Unnamed","dbv":2,"dbid":-1,"authid":-1,"date":"","rxid":0,"rxn":"","rxa":"","rxdb":1,"cr":[],"pub":false,"mo":""}},"gt":2,"wl":3,"q":false,"tl":true,"tea":false,"ga":"b","mo":"b","bal":[]}]</code>
    <br>Items:
    <ol type=1>
      <li>An object with map + other game-related data.</li>
    </ol>
  </p></li>
  <li id="inc26"><p>
    26: Mode Change
    <br>Example: <code>42[26,"b","ard"]</code>
    <br>Items:
    <ol type=1>
      <li>The 'engine' for this mode. Known engines are "b" for most modes and "f" for Football.</li>
      <li>The actual mode. Known modes are:
      <ul>
        <li>"f": Football</li>
        <li>"bs": Simple</li>
        <li>"ard": Death Arrows</li>
        <li>"ar": Arrows</li>
        <li>"sp": Grapple</li>
        <li>"v": VTOL</li>
        <li>"b": Classic</li>
      </ul></li>
    </ol>
  </p></li>
  <li id="inc27"><p>
    27: Change WL (Rounds)
    <br>Example: <code>42[27,7]</code>
    <br>Items:
    <ol type=1>
      <li>The new amount of rounds. (WIN/LOSE) </li>
    </ol>
  </p></li>
  <li id="inc29"><p>
    29: Map switch
    <br>Example: <code>42[29,"ILAMJAhBFBjBzCTlMiAJgNQEYFsCsAFtgOqYDWIhAjLAEyYCeAkgOICcArgFrQr8gACgHpRwgBwpEAWQFyQAXiA"]</code>
    <br>Items:
    <ol type=1>
      <li>An encoded string containing the map. This format will likely be demystified in another file soon.</li>
    </ol>
  </p></li>
    <li id="inc33"><p>
    33: Map Suggest
    <br> (Only host sees this packet other players see <a href="#inc34">Map Suggest Client</a> instead of this packet)
    <br>Example: <code>42[33,"ILAMJAhBFBjBzCTlMiAJgNQEYFsCsAFtgOqYDWIhAjLAEyYCeAkgOICcArgFrQr8gACgHpRwgBwpEAWQFyQAXiA",2]</code>
    <br>Items:
    <ol type=1>
      <li>The MAP Data</li>
      <li>Player ID of player who suggested the map</li>
    </ol>
  </p></li>
   <li id="inc34"><p>
    34: Map Suggest Client
    <br> (Host Sees <a href="#inc52">Map Suggest</a> instead of this packet)
    <br>Example: <code>42[34,"CDball","MuadDib",2]</code>
    <br>Items:
    <ol type=1>
      <li>The Title of the map that was suggested</li>
      <li>The Author of the map that was suggested</li>
      <li>Player ID of player who suggested the map</li>
    </ol>
  </p></li>
  <li id="inc40"><p>
    40: Save Replay
    <br>Example: <code>42[40,1]</code>
    <br>Items:
    <ol type=1>
      <li>The id of a player that tried to save replay</li>
    </ol>
  </p></li>
  <li id="inc43"><p>
    43: Game Starting Countdown
    <br>Example: <code>42[43,3]</code>
    <br>Items:
    <ol type=1>
      <li>Countdown numberr</li>
    </ol>
  </p></li>
  </p></li>
    <li id="inc44"><p>
    44: Abort Countdown
    <br>Example: <code>42[44]</code>
  </p></li>
  <li id="inc46"><p>
    46: Local Gained XP
    <br>Example: <code>42[46,{"newXP":300}]</code>
    <br>Items:
    <ol type=1>
      <li>An object:
      <ul>
        <li>"newXP": Your new XP amount.</li>
        <li>"newLevel" (only if you just leveled up): Your new level.</li>
        <li>"newToken" (only if you just leveled up): The new token to use when preforming operations. Unknown if the old token stops functioning.</li>
      </ul>
    </ol>
  </p></li>
   <li id="inc52"><p>
    52: Tabbed
    <br>Example: <code>42[52,3,false]</code>
    <br>Items:
    <ol type=1>
      <li>Player id of person who tabbed in/out</li>
      <li><code>true</code> if player tabbed in, <code>false</code> if player tabbed out</li>
    </ol>
  </p></li>
  <li id="inc58"><p>
    58: Room Name Update 
    <br>Happens when host changes room name using /roomname "text here"
    <br>Example: host does: /roomname "hello world" result everyone sees: <code>42[58,"hello world"]</code>
    <br>Items:
    <ol type=1>
      <li>new room name</li>
    </ol>
  </p></li>
   <li id="inc59"><p>
    59: Room Pass Update 
    <br>Happens when host changes password name using /roompass "password here" or /clearroompass
    <br>Example: host does: /roomname "1234" result everyone sees: <code>42[59,1]</code>
    <br>Items:
    <ol type=1>
      <li>room Password Update Type (0 Being password was cleared 1 being password was set to something)</a>
    </ol>
  </p></li>
</ul>

_____

## Outgoing

<ul>
  <li id="out3"><p>
    3: Send Inputs 
    <br>Examples:
    <ul>
      <li><code>42[4,{"i":38,"f":324,"c":45}]</code></li>
      <li><code>42[4,{"i":25,"f":531,"c":108}]</code></li>
    </ul>
    <br>Items:
    <ol type=1>
      <li>An object:
      <ul>
        <li>"i": The inputs. Each bit is a direction. Left=1, right=2, up=4, down=8, heavy=16, special=32</li>
        <li>"f": The frame this input was preformed on.</li>
        <li>"c": Sequence number. Starts at 0 and lasts the entire session, increasing by 1 every time the player makes an input.</li>
      </ul>
      </li>
    </ol>
  </p></li>
   <li id="out5"><p>
    5: Trigger Start
    <br>Start the game as a host
    <br>Example: <code>42[5,{"is":"jWCW9ahaqG6GsGbWmycybYaVyafa7GAqc0bXagWWe0agouIGdtGhSKWavaAGefSlIWqacsOcamIjdyjBcFqKukaGe4IUCdirGa1anYcAkl0FguaALYATYgFUoJ8xcFaALCYu4ARsQBiAVkL0AFJYuIIoXABuuADCAIYwAIpcJnBUmt4ALpG4xKhoEUhmWiEAoslqAM55AJ5cAGbO1aD0HN51ohBcZr4AVhBR0UiCJdEmgQB2hAAiHV2EFf0xHjAAzChTDdgmABZsscTAKxQo3hoNrFwWWh7d28u69joU4xQlKU9mpDr1OQxaVPsvj8SiRvGk6vRoIJAs54kl7NUAKbEADsAEYEcjBBi1EjiHAAJbZaIUFbiey4CwwUTVMYZDxgSFccaBCo8XCiIaBCKxIo9KykCoBKRjaoUXBcAC8QA","gs":{"map":"ILAMJAhBFBjBzCTlMiAJgNQEYFsCsAFtgOqYDWIhAKgIYDiAnAMwCaATAGIBeAWtCkEgACgHpxogBwpEAWSEKQAXiA","gt":2,"wl":3,"q":false,"tl":false,"tea":false,"ga":"b","mo":"b","bal":[]}}]</code>
    <br>Items:
    <ol type=1>
      <li>'is': No clue to be honest. This probably contains data like players</a></li></a>
      <li>'gs': (Game settings or game state) An object with map + other game-related data.</li>
    </ol>
  </p></li>
  <li id="out6"><p>
    6: Change Own Team 
    <br>Example: <code>42[6,{"targetTeam":0}]</code>
    <br>Items:
    <ol type=1>
      <li>'targetTeam': The team you will move to. See <a href="#common_schemes">Common Scemes</a></li></a>
    </ol>
  </p></li>
  <li id="out7"><p>
    7: Team Lock
    <br>Example: <code>42[7,{"teamLock":false}]</code>
    <br>Items:
    <ol type=1>
      <li>'teamLock': whether to disable/enable team lock <code>true</code> to lock teams <code>false</code> false to have teams unlocked</li></a>
    </ol>
  </p></li>
  <li id="out9"><p>
    9: Ban Player
    <br>Example: <code>42[9,{"banshortid":6}]</code>
    <br>Items:
    <ol type=1>
      <li>'banshortid': The player's ID.</li></a>
    </ol>
  </p></li>
  <li id="out10"><p>
    10: Chat Message
    <br>Send a chat message
    <br>Example: <code>42[10,{"message":"Hello, World!"}]</code>
    <br>Items:
    <ol type=1>
      <li>'message': The message you want to send</li></a>
    </ol>
  </p></li>
  <li id="out11"><p>
    11: inform In Lobby
    <br>packet send by the host send to joining players to inform them about the game settings
    <br>Example: <code>42[11,{"sid":2,"gs":{"map":{"v":13,"s":{"re":false,"nc":false,"pq":1,"gd":25,"fl":false},"physics":{"shapes":[],"fixtures":[],"bodies":[],"bro":[],"joints":[],"ppm":12},"spawns":[],"capZones":[],"m":{"a":"Showcase","n":"Empty Map","dbv":2,"dbid":767645,"authid":-1,"date":"","rxid":0,"rxn":"","rxa":"","rxdb":1,"cr":["uint32"],"pub":true,"mo":""}},"gt":2,"wl":3,"q":false,"tl":false,"tea":false,"ga":"b","mo":"b","bal":[]}}]</code>
    <br>Items:
    <ol type=1>
      <li>'sid': The sid that will be assigned to that player</li></a>
      <li>'gs': Game settings (Stuff like the map and rounds etc)</li></a>
    </ol>
  </p></li>
  <li id="out12"><p>
    12: create Room
    <br>packet send to create a room
    <br>Example: When Logged in: <code>42[12,{"peerID":"ht1a3nt5tgc00000","roomName":"Showcase's game","maxPlayers":6,"password":"","dbid":12741896,"guest":false,"minLevel":0,"maxLevel":999,"latitude":420.911,"longitude":0.69,"country":"CN","version":44,"hidden":0,"quick":false,"mode":"custom","token":"TOKENHERE","avatar":{"layers":[],"bc":4492031}}]</code><br>
   <br>Example: When Logged out: <code>42[12,{"peerID":"b6sg533lh1v00000","roomName":"net's game","maxPlayers":6,"password":"","dbid":12741896,"guest":true,"minLevel":0,"maxLevel":999,"latitude":666.0,"longitude":0.1234,"country":"CN","version":44,"hidden":0,"quick":false,"mode":"custom","guestName":"net","avatar":{"layers":[],"bc":12634675}}]</code>
    <br>Items:
    <ol type=1>
      <li>'peerID': Your peer id</li>
      <li>'roomName': The room name</li></a>
      <li>'maxPlayers': The max amount of players that can join that room</li></a>
      <li>'password': The room password</li></a>
      <li>'dbid': The room database ID</li></a>
      <li>'guest': Whether you are a quest</li></a>
      <li>'minLevel': The min amount of level you need to be to join that room</li></a>
      <li>'maxLevel': The max amount of level you can be to be able to join that room</li></a>
      <li>'latitude': latitude of your where your room is located</li></a>
      <li>'longitude': longitude of your where your room is located</li></a>
      <li>'country': The country code for your room</li></a>
      <li>'version': Bonk.io Version? </li></a>
      <li>'hidden': Whether the room shows up in the room list</li></a>
      <li>'quick': Whether the room should be created in quickplay</li></a>
      <li>'mode': Room mode. Can consist of these options: <code>bonkquick</code>,<code>arrowsquick</code>,<code>grapplequick</code>,<code>custom</code> </li></a>
      <li>'guestName': What your guestname would be if guest is is set to true</li></a>
      <li>'avatar': Your skin data</li></a>
    </ol>
  </p></li>
    <li id="out14"><p>
    14: Return To Lobby
    <br>Exit out of the game to return to the lobby
    <br>Example: <code>42[14]</code>
  </p></li>
  <li id="out16"><p>
    16: Set Ready
    <br>Enable/Disable the ready checkmark
    <br>Example: <code>42[16,{"ready":false}]</code>
    <br>Items:
    <ol type=1>
      <li>"ready": <code>true</code> if you want to have be ready (have a checkmark), otherwise <code>false</code>.    </ol>
  </p></li>
  <li id="out20"><p>
    20: send GAMO
    <br>packet Send to change the rooms mode
    <br>Example: <code>42[20,{"ga":"b","mo":"ar"}]	</code>
    <br>Items:
    <ol type=1>
      <li>'ga': The 'engine' for this mode. Known engines are "b" for most modes and "f" for Football.</li>
      <li>'mo': The actual mode. Known modes are:
      <ul>
        <li>"f": Football</li>
        <li>"bs": Simple</li>
        <li>"ard": Death Arrows</li>
        <li>"ar": Arrows</li>
        <li>"sp": Grapple</li>
        <li>"v": VTOL</li>
        <li>"b": Classic</li>
      </ul></li>
    </ol>
  </p></li>
  <li id="out21"><p>
    21: send WL (Rounds)
    <br>set the amount of rounds to win (Win/Lose)
    <br>Example: <code>42[21,{"w":6}]	</code>
    <br>Items:
    <ol type=1>
      <li>"w": The amount of rounds</ol>
  </p></li>
  <li id="out23"><p>
    23: send Map Add
    <br>Change the current map
    <br>Example: <code>42[23,{"m":"ILAMJAhBFBjBzCTlMiArAFQFoA0AW6AkgKIBqALrABIBKxJAjPrCAHIBGjbdJANgGk2AERIAvbAFsAYpOwoFJYMIDqATky1QtAFIBlAFbQAHgFkATNnxToSADIUATgFU0SRKYVekAXiA"}] </code>
    <br>Items:
    <ol type=1>
      <li>"m": The Map Data</ol>
  </p></li>
  <li id="out26"><p>
    26: Change Other Team 
    <br>Example: <code42[26,{"targetID":1,"targetTeam":1}]	</code>
    <br>Items:
    <ol type=1>
      <li>'targetID': The Players ID of who you are moving</li></a>
      <li>'targetTeam': The team the person will be moved to. See <a href="#common_schemes">Common Scemes</a></li></a>
    </ol>
  </p></li>
  <li id="out27"><p>
    27: send Map Suggest
    <br>Example: <code42[27,{"m":"ILAMJAhBFBjBzCTlMiAHgEQCoCYCcAzgIYDqAHPAEakBiA7mecAMIC2tALlbgCYCMsfgDkAqtABqASRQo0wAA4ALGvgB2vABrDsEgKK0AykhYB2AFYBpFDMyz7SIA","mapname":"COolio mapio","mapauthor":"amogusSTAR"}]</code>
    <br>Items:
    <ol type=1>
      <li>'m': The Map Data</li></a>
      <li>'mapname': The Maps Name</li></a>
      <li>'mapname': The Maps Author (Map Creator)</li></a>
    </ol>
  </p></li>
  <li id="out29"><p>
    29: send Balance
    <br>Change a players nerf/buff
    <br>Example: <code42[29,{"sid":2,"bal":-55}]</code>
    <br>Items:
    <ol type=1>
      <li>'sid': The Players ID of who you are adding the balance to</li></a>
      <li>'bal': The balance amont</li></a>
    </ol>
  </p></li>
  <li id="out32"><p>
    32: send Team Settings Change
    <br>Enable/Disable teams
    <br>Example: <code42[32,{"t":true}]</code>
    <br>Items:
    <ol type=1>
      <li>"t": <code>true</code> if teams should be on, otherwise it should be <code>false</code>.</li>
    </ol>
  </p></li>
  <li id="out33"><p>
    33: send Arm Record
    <br>Save a replay
    <br>Example: <code42[33]</code>
  </p></li>
 </ul>

