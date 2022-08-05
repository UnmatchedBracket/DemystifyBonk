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



## Incoming
Contents:
<ul>
<li><a href="#inc1">1: Ping</a></li>
<li><a href="#inc3">3: Room join</a></li>
<li><a href="#inc4">4: Player join</a></li>
<li><a href="#inc5">5: Player leave</a></li>
<li><a href="#inc6">6: Host leave</a></li>
<li><a href="#inc7">7: Movement</a></li>
<li><a href="#inc8">8: [READY] enable/disable</a></li>
<li><a href="#inc13">13: Game End</a></li>
<li><a href="#inc15">15: Game Start</a></li>
<li><a href="#inc16">16: Error</a></li>
<li><a href="#inc18">18: Team Change</a></li>
<li><a href="#inc19">19: Teamlock toggle</a></li>
<li><a href="#inc20">20: Chat</a></li>
<li><a href="#inc21">21: Initial data</a></li>
<li><a href="#inc26">26: Mode change</a></li>
<li><a href="#inc27">27: Round count change</a></li>
<li><a href="#inc29">29: Map switch</a></li>
</ul>
<ul>
  <li id="inc1"><p>
    1: Ping
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
    7: Movement
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
    8: [READY] enable/disable
    <br>Example: <code>42[8,1,true]</code>
    <br>Items:
    <ol type=1>
      <li>The ID of the person that changed their [READY] status</li>
      <li><code>true</code> if the player enabled [READY], <code>false</code> if they disabled it.</li>
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
        <li>not_hosting: You attempted to do an action that requires you to be the game's host.</li>
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
    20: Chat
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
    27: Round count change
    <br>Example: <code>42[27,7]</code>
    <br>Items:
    <ol type=1>
      <li>The new amount of rounds.</li>
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
</ul>
