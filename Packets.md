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
      <li>A list of players, starting at ID 0 and increasing by 1 each item. <code>null</code> indicates that player left.</li>
      <li>Server Unix timestamp??</li>
      <li><code>true</code> if teams are locked, otherwise <code>false</code>.</li>
      <li>Room ID.</li>
      <li>Room bypass. Use with the Room ID to get the share link (https://bonk.io/&lt;room id right-padded to six digits&gt;&lt;room bypass&gt;)</li>
      <li>I have no clue what this is. AFAIK it's always <code>null</code></li>
    </ol>
  </p></li>
</ul>
