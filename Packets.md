# Packets
If you've connected to a bonk.io server, these are the packets to use.

Each packet usually starts with "42" and then has a JSON list. The first item in the list specifies what type of packet it is.

## Incoming
<ul>
  <li><p>
    1: A ping packet.
    <br>This packet is used to provide info about other player's pings, as well as to measure your own.
    <br>Example: <code>42[1,{"30":180,"33":148,"34":190},9]</code>
    <br>Items:
    <ol type=1>
      <li>An object with the other user's ping times.</li>
      <li>An number to determine which ping a client is responding to. The client should echo <code>42[1,{"id":<this number>}]</code> when this packet is recieved.</li>
    </ol>
  </p></li>
  <li><p>
    3: Join packet.
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
        <li>"team": The player's team. 0=Spectator, 1=FFA, 2=Red, 3=Blue, 4=Green, 5=Yellow</li>
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
  <li><p>
    4: Player join packet.
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
  <li><p>
    5: Player leave packet.
    <br>Example: <code>42[5,13,14511]</code>
    <br>Items:
    <ol type=1>
      <li>The player's ID.</li>
      <li>The tick on which they left?</li>
    </ol>
  </p></li>
  <li><p>
    6: Host leave packet.
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
  <li><p>
    7: Movement packet.
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
</ul>
