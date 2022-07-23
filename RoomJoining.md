# How to Join a Room
Let's say you have yourself a room link (like <code>https://bonk.io/233487vpoqf</code>) and you want to join that room. Here are the steps which you must preform:

<ol>
<li>Fetch that URL
<br>The HTML of that join URL contains a &lt;script&gt; tag like this:
<br><code>
&lt;script&gt;
document.getElementById('maingameframe').contentWindow.autoJoin = {"address":"QzyOlm5FvB4XcOjAEhCk","roomname":"user one's game","server":"b2seattle1","passbypass":"vpoqf","r":"success"};&lt;/script&gt;
</code>
<br>The important parts are "address", "server", and "passbypass". Future steps will have parts like <code>{address}</code>, replace those parts with the corresponding item in this object.
</li>
<li>Get a Peer ID
<br>Make a GET request to <code>https://{server}.bonk.io/myapp/peerjs/id</code>. The response (as text) is your Peer ID. Replace <code>{peerID}</code> in future steps with this ID.</li>
<li>Get a SID
<br>First, you'll need a yeast. This yeast marks the current time, and I have no clue why you need it but you do. Refer to the following peice of Python code:
<br><pre><code>alphabet = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz-_"
def yeast():
    num = math.floor(time.time()*1000)
    encoded = ""
    while num > 0 or encoded == "":
        encoded = alphabet[num % len(alphabet)] + encoded
        num = math.floor(num / len(alphabet))
    return encoded
</code></pre>
Once you have a yeast as <code>{yeast}</code>,
make a GET request to <code>https://{server}.bonk.io/socket.io/?EIO=3&transport=polling&t={yeast}</code>

The response looks like this: <code>96:0{"sid":"G233o-1HCctoNKD_PVHZ","upgrades":["websocket"],"pingInterval":25000,"pingTimeout":5000}</code>

The only important part is the "sid", so grab that and move on.
</li>
<li>Log in (optional)
<br>To log in, make a POST request to <code>https://bonk2.io/scripts/login_legacy.php</code> with the POST data of <code>username={username}&password={password}&remember=false</code> and the header <code>content-type</code> set to <code>application/x-www-form-urlencoded; charset=UTF-8</code>
<br>The response is a large JSON, but all you need is the "token" key.
</li>
<li>The Websocket
<br>It's finally time to connect to the server. Start a WebSocket connection to <code>wss://{server}.bonk.io/socket.io/?EIO=3&transport=websocket&sid={sid}</code>
<br>Then, send <code>2probe</code>. The server should respond with <code>3probe</code>.
Send <code>5</code>. Again, the server should respond with <code>40</code>.
<br>Finally, send one of the following packets:
<ul>
<li>If logged in:
<br><code>42[13,{"joinID":"{address}","avatar":{"layers":[],"bc":0},"guest":false,"token":{token},"dbid":2,"version":44,"peerID":"{peerID}","bypass":"{passbypass}"}]</code></li>
<li>If not logged in:
<br><code>42[13,{"joinID":"{address}","avatar":{"layers":[],"bc":0},"guest":true,"dbid":2,"version":44,"peerID":"{peerID}","bypass":"{passbypass}","guestName":"testusername"}}]</code></li>
</ul>
You should then have joined the lobby!
Afterwards, make sure to send <code>2</code> every 10 seconds or so to tell the server you're still there.
</li>
</ol>