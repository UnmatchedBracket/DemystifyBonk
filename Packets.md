# Packets
If you've connected to a bonk.io server, these are the packets to use.

Each packet usually starts with "42" and then has a JSON list. The first item in the list specifies what type of packet it is.

## Incoming
<ul>
  <li><p>
    1: A ping packet.
    <br>This packet is used to provide info about other player's pings, as well as to measure your own.
    <br>Example: <code>42[1,{"30":180,"33":148,"34":190},9]</code>
    <ol type=1>
      <li>An object with the other user's ping times.</li>
      <li>An number to determine which ping a client is responding to. The client should echo <code>42[1,{"id":<this number>}]</code> when this packet is recieved.</li>
    </ol>
  </p></li>
</ul>
