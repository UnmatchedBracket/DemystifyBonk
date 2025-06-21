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
<li><a href="#inc9">9: All Ready Reset</a></li>
<li><a href="#inc13">13: Game End</a></li>
<li><a href="#inc15">15: Game Start</a></li>
<li><a href="#inc16">16: Status Message</a></li>
<li><a href="#inc18">18: Team Change</a></li>
<li><a href="#inc19">19: Teamlock toggle</a></li>
<li><a href="#inc20">20: Chat Message</a></li>
<li><a href="#inc21">21: Initial data</a></li>
<li><a href="#inc25">25: Map Reorder</a></li>
<li><a href="#inc26">26: Mode change</a></li>
<li><a href="#inc27">27: Change WL (Rounds)</a></li>
<li><a href="#inc29">29: Map switch</a></li>
<li><a href="#inc32">32: Afk Warn</a></li>
<li><a href="#inc33">33: Map Suggest</a></li>
<li><a href="#inc34">34: Map Suggest Client</a></li>
<li><a href="#inc36">36: Balance Set</a></li>
<li><a href="#inc40">40: Save Replay</a></li>
<li><a href="#inc42">42: Friend Req</a></li>
<li><a href="#inc43">43: Game starting Countdown</a></li>
<li><a href="#inc44">44: Abort Countdown</a></li>
<li><a href="#inc45">45: Player Leveled Up</a></li>
<li><a href="#inc46">46: Local Gained XP</a></li>
<li><a href="#inc49">49: Created Room Share Link</a></li>
<li><a href="#inc52">52: Tabbed</a></li>
<li><a href="#inc57">57: Curate Result</a></li>
<li><a href="#inc58">58: Room Name Update</a></li>
<li><a href="#inc59">59: Room Password Update</a></li> 
</ul>

### Outgoing
<ul>
<li><a href="#out4">4: Send Inputs</a></li> 
<li><a href="#out5">5: Trigger Start</a></li> 
<li><a href="#out6">6: Change Own Team</a></li> 
<li><a href="#out7">7: Team Lock</a></li> 
<li><a href="#out9">9: Kick/Ban Player</a></li> 
<li><a href="#out10">10: Chat Message</a></li>
<li><a href="#out11">11: Inform In Lobby</a></li> 
<li><a href="#out12">12: Create Room</a></li> 
<li><a href="#out14">14: Return To Lobby</a></li> 
<li><a href="#out16">16: Set Ready</a></li> 
<li><a href="#out17">17: All Ready Reset</a></li> 
<li><a href="#out19">19: Send Map Reorder</a></li> 
<li><a href="#out20">20: Send Mode</a></li> 
<li><a href="#out21">21: Send WL (Rounds)</a></li> 
<li><a href="#out22">22: Send Map Delete</a></li> 
<li><a href="#out23">23: Send Map Add</a></li> 
<li><a href="#out26">26: Change Other Team</a></li> 
<li><a href="#out27">27: Send Map Suggest</a></li> 
<li><a href="#out29">29: Send Balance</a></li> 
<li><a href="#out32">32: Send Team Settings Change</a></li> 
<li><a href="#out33">33: Send Arm Record</a></li> 
<li><a href="#out34">34: Send Host Change</a></li> 
<li><a href="#out35">35: Send Friended</a></li> 
<li><a href="#out36">36: Send Start Countdown</a></li> 
<li><a href="#out37">37: Send Abort Countdown</a></li> 
<li><a href="#out38">38: Send Req XP</a></li> 
<li><a href="#out39">39: Send Map Vote</a></li> 
<li><a href="#out39">40: Inform In Game</a></li> 
<li><a href="#out41">41:  Get Pre Vote</a></li> 
<li><a href="#out44">44: Tabbed</a></li> 
<li><a href="#out50">50: Send No Host Swap</a></li> 
<li><a href="#out50">51: Send Curate</a></li> 
<li><a href="#out51">52: Room Name Update</a></li> 
<li><a href="#out52">53: Room Password Update</a></li> 
</ul>
	
### Outgoing Debug
*Possibly unused/Debug Outgoing Packets*
<ul>
<li><a href="#debugout2">2: Test Ping</a></li> 
<li><a href="#debugout3">3: Get Debug</a></li> 
<li><a href="#debugout8">8: Silence Player</a></li> 
<li><a href="#debugout24">24: Send Typing</a></li> 
<li><a href="#debugout30">30: Version Check</a></li> 
<li><a href="#debugout31">31: Send Debug Winner</a></li> 
<li><a href="#debugout45">45: Desync Test</a></li> 
<li><a href="#debugout46">46: Send Desync Res</a></li> 
</ul>

### Incoming Debug/Unused
*Possibly unused/Debug Incoming Packets*
<ul>
<li><a href="#debuginc30">30: Typing</a></li> 
<li><a href="#debuginc30">38: Debug Winner</a></li> 
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
      <li>If the player is joined via bypass</li>
      <li>A JSON object representing the new player's skin.</li>
    </ol>
  </p></li>
  <li id="inc5"><p>
    5: Player leave
    <br>Example: <code>42[5,13,14511]</code>
    <br>Items:
    <ol type=1>
      <li>The player's ID.</li>
      <li>The tick on which they left? / Accumulator?</li>
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
      <li>?</li>
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
  <li id="inc9"><p>
    9: All Ready Reset
    <br>Example: <code>42[9]</code>
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
      <li>Map Data encoded with LZ-String</li>
      <li>An object with map + other game-related data.</li>
    </ol>
  </p></li>
  <li id="inc16"><p>
    16: Status Message
    <br>Example: <code>42[16,"rate_limit_ready"]</code>
    <br>Items:
    <ol type=1>
      <li>The status codes. List (mostly complete):
      <ul>
        <li>arm rate limited: You spammed save replay too fast</li>
	<li>room_not_found: You tried to join a room that doesn't exist anymore</li>
        <li>room_full: You tried to join a full room.</li>
        <li>banned: You tried to join a room you've been kicked from.</li>
	<li>Initial data timeout.: </li>
	<li>Connect error: </li>
	<li>old_rotation: Quick play related</li>
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
        <li>rate_limit_sgt: You changed modes too quickly.</li>
	<li>rate_limit_rtl: You sent the return to lobby packet too quickly.</li>
	<li>rate_limit_pong: You sent the ping packet too quickly.</li>
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
      <li>An object containing the map data:
    </ol>
  </p></li>
 <li id="inc25"><p>
    25: Map Reorder
    <br>Example: <code>NO EXAMPLE</code>
    <br>Items:
    <ol type=1>
      <li>Start</li>
      <li>End</li>
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
      <li>An LZ-String encoded string containing the map. This format will likely be demystified in another file soon.</li>
    </ol>
  </p></li>
  <li id="inc32"><p>
    32: Afk Warn
    <br>When you are about to be disconnected for being afk this is send to u
    <br>Example: <code>42[32]</code>
    <br>Items:
    <ol type=1>
      <li>The LZ-String encoded version of the map data</li>
      <li>Player ID of player who suggested the map</li>
    </ol>
  </p></li>
  <li id="inc33"><p>
    33: Map Suggest
    <br> (Only host sees this packet other players see <a href="#inc34">Map Suggest Client</a> instead of this packet)
    <br>Example: <code>42[33,"ILAMJAhBFBjBzCTlMiAJgNQEYFsCsAFtgOqYDWIhAjLAEyYCeAkgOICcArgFrQr8gACgHpRwgBwpEAWQFyQAXiA",2]</code>
    <br>Items:
    <ol type=1>
      <li>The LZ-String encoded version of the map data</li>
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
  <li id="inc36"><p>
    36: Balance Set
    <br>Example: <code>42[36,1,-55]</code>
    <br>Items:
    <ol type=1>
      <li>Player ID of player whos balance was changed</li>
      <li>The balance abount, as an integer from -100 to 100.</li>
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
  <li id="inc42"><p>
    42: Friend Req
    <br>Example: <code>42[42,1]</code>
    <br>Items:
    <ol type=1>
      <li>The id of a player that send you a friend request</li>
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
  <li id="inc44"><p>
    44: Abort Countdown
    <br>Example: <code>42[44]</code>
  </p></li>
  <li id="inc45"><p>
    45: Player Leveled Up
    <br>Example: <code>42[45,{"sid":1,"lv":69}]</code>
    <br>Items:
    <ol type=1>
      <li>An object:
      <ul>
        <li>"sid": The Player that leveled up.</li>
        <li>"lv": Their New Level.</li>
      </ul>
    </ol>
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
  <li id="inc49"><p>
    49: created Room
    <br>Example: <code>42[49,261254,"agsey"]</code>
    <br>Items:
    <ol type=1>
      <li>The new DBId for the room (Aka the id at the end of a invite link for example https://bonk.io/12345)</li>
      <li>The room bypass, if any. Otherwise, this is "".</li>
    </ol>
  </p></li>
   <li id="inc52"><p>
    52: Tabbed
    <br>Also known as AFK Status
    <br>Example: <code>42[52,3,false]</code>
    <br>Items:
    <ol type=1>
      <li>Player id of person who tabbed in/out</li>
      <li><code>false</code> if the player is now focused on the tab, otherwise <code>true</code>.</li>
  </ol>
  </p></li>
  <li id="inc57"><p>
    57: Curate Result
    <br>Result from /curateyes (related to /curate MESSAGEHERE)
    <br>Example: unauthorized fail: <code>42[57,false,"unauthorised"]</code> 
    <br>Unknown error fail <code>42[57,false,""]</code>
    <br>Succeed <code>42[57,true,""]</code>
    <br>Items:
    <ol type=1>
      <li>Whether curation succeeded</li>
      <li>Error / Status message </li>
    </ol>
    <ol type=1>
      <li>The status / error codes:</li>
      <ul>
        <li>rate_limit: Rate limited, please wait a few seconds then try /curateyes again</li>
	<li>not_logged_in: You're a guest!</li>
        <li>invalid_mapid: Map issue</li>
        <li>invalid_comment: Comment invalid</li>
	<li>comment_too_long: Comment too long</li>
	<li>invalid_dbv: Database version issue / map issue</li>
	<li>unauthorised: You don't have those privileges</li>
	<li>map_private: Map is private, it must be published</li>
        <li>: unknown error</li>
      </ul>
      </li>
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
    <br>Example: host does: /roompass "1234" result everyone sees: <code>42[59,1]</code>
    <br>Items:
    <ol type=1>
      <li>room Password Update Type (0 Being password was cleared 1 being password was set to something)</a>
    </ol>
  </p></li>
</ul>

_____

## Outgoing

<ul>
  <li id="out4"><p>
    4: Send Inputs 
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
    9: Kick/Ban Player
    <br>Example: <code>42[9,{"banshortid":6}]</code>
    <br>Items:
    <ol type=1>
      <li>'banshortid': The player's ID.</li></a>
      <li>'kickonly': Whether the player can join back after being kicked <code>true</code> the player will be able to join back <code>false</code> the player wont be able to join back</li></a>
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
    11: Inform In Lobby
    <br>packet send by the host send to joining players to inform them about the game settings
    <br>Example: <code>42[11,{"sid":2,"gs":{"map":{"v":13,"s":{"re":false,"nc":false,"pq":1,"gd":25,"fl":false},"physics":{"shapes":[],"fixtures":[],"bodies":[],"bro":[],"joints":[],"ppm":12},"spawns":[],"capZones":[],"m":{"a":"Showcase","n":"Empty Map","dbv":2,"dbid":767645,"authid":-1,"date":"","rxid":0,"rxn":"","rxa":"","rxdb":1,"cr":["uint32"],"pub":true,"mo":""}},"gt":2,"wl":3,"q":false,"tl":false,"tea":false,"ga":"b","mo":"b","bal":[]}}]</code>
    <br>Items:
    <ol type=1>
      <li>'sid': The sid that will be assigned to that player</li></a>
      <li>'gs': Game settings (Stuff like the map and rounds etc)</li></a>
    </ol>
  </p></li>
  <li id="out12"><p>
    12: Create Room
    <br>packet send to create a room
    <br>Example: When Logged in: <code>42[12,{"peerID":"ht1a3nt5tgc00000","roomName":"Showcase's game","maxPlayers":6,"password":"","dbid":12741896,"guest":false,"minLevel":0,"maxLevel":999,"latitude":420.911,"longitude":0.69,"country":"CN","version":45,"hidden":0,"quick":false,"mode":"custom","token":"TOKENHERE","avatar":{"layers":[],"bc":4492031}}]</code><br>
   <br>Example: When Logged out: <code>42[12,{"peerID":"b6sg533lh1v00000","roomName":"net's game","maxPlayers":6,"password":"","dbid":12741896,"guest":true,"minLevel":0,"maxLevel":999,"latitude":666.0,"longitude":0.1234,"country":"CN","version":45,"hidden":0,"quick":false,"mode":"custom","guestName":"net","avatar":{"layers":[],"bc":12634675}}]</code>
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
      <li>'version': Bonk.io Version</li></a>
      <li>'hidden': Whether the room shows up in the room list</li></a>
      <li>'quick': Whether the room should be created in quickplay</li></a>
      <li>'mode': Room mode. Can consist of these options: <code>bonkquick</code>,<code>arrowsquick</code>,<code>grapplequick</code>,<code>custom</code> </li></a>
      <li>'guestName': What your guestname would be if guest is is set to true</li></a>
      <li>'avatar': Your skin data</li></a>
      <li>'game': This should only be added when you want to create a room for supercarstadium. and in that case you want game to be "car"</li></a>
    </ol>
  </p></li>
    <li id="out14"><p>
    14: Return To Lobby
    <br>Exit out of the game to return to the lobby.
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
  <li id="out17"><p>
    17: All Ready Reset
    <br>The host can send this packet to set everyones ready status to false.
    <br>Example: <code>42[17]</code>
  </li>
  <li id="out19"><p>
    19: Send Map Reorder
    <br>Example: No Example</code>
    <br>Items:
    <ol type=1>
      <li>"s": start</li>
      <li>"e": end</li>
    </ol>
  </p></li>
  <li id="out20"><p>
    20: Send Mode
    <br>Changes the room's mode.
    <br>Example: <code>42[20,{"ga":"b","mo":"ar"}]</code>
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
    21: Send WL (Rounds)
    <br>Sets the amount of rounds to win.
    <br>Example: <code>42[21,{"w":6}]</code>
    <br>Items:
    <ol type=1>
      <li>"w": The amount of rounds</ol>
  </p></li>
  <li id="out22"><p>
    23: Send Map Delete
    <br>Needs Documentation
    <br>Example: No Example
    <br>Items:
    <ol type=1>
      <li>"d": ?</li>
    </ol>
  </p>
  <li id="out23"><p>
    23: Send Map Add
    <br>Changes the current map.
    <br>Example: <code>42[23,{"m":"ILAMJAhBFBjBzCTlMiArAFQFoA0AW6AkgKIBqALrABIBKxJAjPrCAHIBGjbdJANgGk2AERIAvbAFsAYpOwoFJYMIDqATky1QtAFIBlAFbQAHgFkATNnxToSADIUATgFU0SRKYVekAXiA"}]</code>
    <br>Items:
    <ol type=1>
      <li>"m": The Map Data</ol>
  </p></li>
  <li id="out26"><p>
    26: Change Other Team 
    <br>Example: <code>42[26,{"targetID":1,"targetTeam":1}]	</code>
    <br>Items:
    <ol type=1>
      <li>'targetID': The Players ID of who you are moving</li></a>
      <li>'targetTeam': The team the person will be moved to. See <a href="#common_schemes">Common Scemes</a></li></a>
    </ol>
  </p></li>
  <li id="out27"><p>
    27: Send Map Suggest
    <br>Example: <code>42[27,{"m":"ILAMJAhBFBjBzCTlMiAHgEQCoCYCcAzgIYDqAHPAEakBiA7mecAMIC2tALlbgCYCMsfgDkAqtABqASRQo0wAA4ALGvgB2vABrDsEgKK0AykhYB2AFYBpFDMyz7SIA","mapname":"COolio mapio","mapauthor":"amogusSTAR"}]</code>
    <br>Items:
    <ol type=1>
      <li>'m': The map's encoded data</li></a>
      <li>'mapname': The map's name</li></a>
      <li>'mapname': The map's author (Map Creator)</li></a>
    </ol>
  </p></li>
  <li id="out29"><p>
    29: Send Balance
    <br>Change a players nerf/buff.
    <br>Example: <code>42[29,{"sid":2,"bal":-55}]</code>
    <br>Items:
    <ol type=1>
      <li>'sid': The Players ID of who you are adding the balance to</li></a>
      <li>'bal': The balance amount</li></a>
    </ol>
  </p></li>
  <li id="out32"><p>
    32: Send Team Settings Change
    <br>Enable/Disable teams
    <br>Example: <code>42[32,{"t":true}]</code>
    <br>Items:
    <ol type=1>
      <li>"t": <code>true</code> if teams should be on, otherwise it should be <code>false</code>.</li>
    </ol>
  </p></li>
  <li id="out33"><p>
    33: Send Arm Record
    <br>Save a replay
    <br>Example: <code>42[33]</code>
  </p></li>
  <li id="out34"><p>
    34: Send Host Change
    <br>Give Host to someone
    <br>Example: <code>42[34,{"id":1}]</code>
    <ol type=1>
      <li>"id": The persons ID of who will be receiving host</li>
    </ol>
  </p></li>
  <li id="out35"><p>
    35: Send Friended
    <br>send a friend request to someone
    <br>Example: <code>42[35,{"id":5}]</code>
    <ol type=1>
      <li>"id": The persons ID of who you are sending a friend request to</li>
    </ol>
  </p></li>
  <li id="out36"><p>
    36: Send Start Countdown
    <br>Send the message saying: Game Starting in X. 
    <br>Example: <code>42[36,{"num":3}]</code>
    <ol type=1>
      <li>"num": The number in "Game starting in NUMBER...."</li>
    </ol>
  </p></li>
  <li id="out37"><p>
    37: Send Abort Countdown
    <br>Send the message saying: countdown aboted
    <br>Example: <code>42[37]</code>
  </p></li>
  <li id="out38"><p>
    38: Send Request Xp
    <br>This packet is usually send after a round. but sending this packet no matter while you are in the lobby or not will make you gain 100 xp
    <br>Example: <code>42[38]</code>
  </p></li>
  <li id="out39"><p>
    39: Send Map Vote
    <br>Vote up/down on a map
    <br>Example: <code>42[39,{"mapid":98062,"vote":1}]</code>
    <ol type=1>
      <li>"mapid": The Map ID you are voting</li>
      <li>"vote": The type of vote. 1 for thumbs up, 0 for thumbs down</li>
    </ol>
  </p></li>
  <li id="out40"><p>
    40: Inform In Game
    <br>Needs documentation
    <br>Example: <code>42[40,{"sid":1,"allData":{"state":"jWCW9ahaqG6GsGbWmycybYaVyafa7GAqc0bXagWWe0agouIGdtGhSKWavaAGefSlIWqacsOcamIjdyjBcFqKukaGe4IUCdirGa1anYcAkl0FguaALYATYgFUoJ8xcFaALCYu4ARsQBiAVkL0AFJYuIIoXABuuADCAIYwAIpcJnBUmt4ALpG4xKhoEUhmWiEAoslqAM55AJ5cAGbO1aD0HN51ohBcZr4AVhBR0UiCJdEmgQB2hAAiHV2EFf0xHjAAzChTDdgmABZsscTAKxQo3hoNrFwWWh7d28u69joU4xQlKU9mpDr1OQxaVPsvj8SiRvGk6vRoIJAs54kl7NUAKbEADsAEYEcjBBi1EjiHAAJbZaIUFbiey4CwwUTVMYZDxgSFccaBCo8XCiIaBCKxIo9KykCoBKRjaoUXBcAC8QA","stateID":1,"fc":7,"inputs":[],"admin":[],"gs":{"map":"ILAMJAhBFBjBzCTlMiAJgNQEYFsCsAFtgOqYDWIhAKgIYDiAnAMwCaATAGIBeAWtCkEgACgHpxogBwpEAWSEKQAXiA","gt":2,"wl":9,"q":"custom","tl":false,"tea":false,"ga":"b","mo":"sp","bal":]},"random":[]}}]</code>
    <ol type=1>
      <li>"sid": Which player to inform about the ingame data</li>
      <li>"allData": ?</li>
    </ol>
  </p></li>
  <li id="out41"><p>
    41: get Pre Vote
    <br>This packet is send upon a map starting. Needs documentation
    <br>Example: 42[41,{"mapid":831011}]</code>
    <ol type=1>
      <li>"mapid": The map id of the map that started</li>
    </ol>
  </p></li>
  <li id="out44"><p>
    44: Tabbed
    <br>Also known as AFK Status
    <br>Example: <code>42[44,{"out":true}]</code>
    <ol type=1>
      <li>"out": <code>true</code> to set your status as focused on the tab, otherwise <code>false</code>.</li>
    </ol>
  </p></li>
  <li id="out50"><p>
    50: No Host Swap
    <br>Makes it so that when the host leaves the room, the room ends.
    <br>Example: <code>42[50]</code>
  </p></li>
   <li id="out51"><p>
    51: Send Curate
    <br>curate by typing /curate yourmessage then /curateyes to confirm
    <br>Example: <code>42[51,{"mapid":996496,"dbv":1,"comment":"llolololololo"}]</code>
    <ol type=1>
      <li>"mapid": The Map Id</li>
      <li>"dbv": Database Version</li>
      <li>"comment": The comment you entered</li>
    </ol>
  </p></li>
   <li id="out52"><p>
    52: Room Name Update
    <br>Sent when host changes room name using /roomname "text here"
    <br>Example: <code>42[52,{"newName":"text here"}]</code>
    <ol type=1>
      <li>"newName": The new room name</li>
    </ol>
  </p></li>
   <li id="out53"><p>
    53: Room Password Update
    <br>Sent when host changes room password using /roompass "password here" or when the host uses /clearroompass
    <br>Example: <code>42[53,{"newPass":"password here"}]</code>
    <ol type=1>
      <li>"newPass": The new room password (empty if /clearroompass is used)</li>
    </ol>
  </p></li>
  <br><br>
  <i>Possibly unused/Debug Outgoing Packets</i>
  <li id="debugout2"><p>
    2: Test Ping
    <br>Example: <code>42[2]</code>
  </p></li>
  <li id="debugout3"><p>
    3: Get Debug
    <br>Example: <code>42[3]</code>
  </p></li>
  <li id="debugout8"><p>
    8: Silence Player
    <br>Example: <code>NO EXAMPLE</code>
    <ol type=1>
      <li>"muteID": The Player you are muting?</li>
      <li>"muteType": ?</li>
      <li>"action":?</li>
    </ol>
  </p></li>
  <li id="debugout24"><p>
    24: Send Typing
    <br>Example: <code>NO EXAMPLE</code>
  </p></li>
  <li id="debugout30"><p>
    30: Version Check
    <br>Example: <code>42[30]</code>
  </p></li>
  <li id="debugout31"><p>
    31: Send Debug Winner
    <br>Example: <code>42[31,{"wid":0}]</code>
    <ol type=1>
      <li>"wid": Winner Id</li>
    </ol>
  </p></li>
  <li id="debugout45"><p>
    45: Desync Test
    <br>Test whether a player is desynced?
    <br>Example: <code>NO EXAMPLE</code>
    <ol type=1>
      <li>"id": The players id to test whether they are desynced>?</li>
      <li>"a": accumulator?</li>
    </ol>
  </p></li>
  <li id="debugout46"><p>
    46: Send Desync Response
    <br>Example: <code>NO EXAMPLE</code>
    <ol type=1>
      <li>"rid": ?</li>
      <li>"sid": Session Id</li>
      <li>"s": ?</li>
      <li>"a": accumulator?</li>
    </ol>
  </p></li>
  <i>Possibly unused/Debug Incoming Packets</i>
  <li id="debuginc30"><p>
    30: Typing
    <br>Received when a player is typing, this is unused in bonk 
    <br>Example: <code>NO EXAMPLE</code>
    <ol type=1>
      <li>Player Id of who is typing?</li>
    </ol>
  </p></li>
  <li id="debuginc38"><p>
    38: Debug Winner
    <br>Example: <code>NO EXAMPLE</code>
    <ol type=1>
      <li>?</li>
      <li>?</li>   
    </ol>
  </p></li>
 </ul>
