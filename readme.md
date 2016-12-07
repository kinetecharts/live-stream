# Live data stream set up

## osc server end point
live-stream.kinetecharts.org:3334

## data subscription end point
http://live-stream.kinetecharts.org:8080


max/msp patch for sending OSC (by Maarten)
<pre><code>
----------begin_max5_patcher----------
646.3ocyWFtaaBCDG+yTo9NX4OmlfMjDxdUllpLvoD5.ajsIMUU6ceFanMIM
TnrtDDRNx1G3+2u6rOmWu+NObr3.nvnef9Ixy6UyHd1wpGwqc.ObA6PRNSYM
DGWo0BNdVyb7phLdNnsSRZGMK0ZqH9oGnzisUTo+fwkLcxtL91GkPh1IlfUy
8mgBhpaogssne09JtOi9kRvYONlw2heedk9kb6TXrcj+b+c0+Z9Y1fczDQQA
v0uodMbvpNLbfUTlCnBPoXaAzWfE9cvB+OiEqCpAvRKRHK8szv+XZ7c3sb3Y
iB+nypQwn3g6gjnN7PZuQ6UtXtKZS6OZO6+RTuKNjKXotkcvnX8nS7IVHrxe
nn36EAe0c3jUiwQIqsY092xs3M6f+XzdQAiI0.ewuy3jEPdr34GkHxbBs94M
6KkfxbDASmI3mDD23hh1Mt9MMuK8SHJ8RDMbLD0OxsptTnH5.xdtk7L2vSZP
urLvBQhOcrrLXDrjtl5V0ktCcCmPrrTH00XIrOzEFZwUf6f0wftwW5lzblNY
BAtcBkFkmsGdPok.qXtIYDzPxNSpoZtP1wY6WjLjQPlkjiyohtA4TcUbqJsz
j.khHTygxlGxo4W8VAXyXtViMAgRZNp7B0551ic1fyMwuyu8pckpm3LNnDUx
jVr1VdFczpkBJcF2tG5Hqpu85wVsKKME3m3aoYJVbNXoge2glAKryWxKKr5K
ZckEVcRe+Bay0WWzIptBln5JbhpqnAk3SuMBizqvHWegsZPDKbpJrf+Ig0TE
fUVtGjplOsSSlxdOIj08WOy0Oi65G35Kg8YsuhqhFlIMknLWGPWIcUcND07G
avEhTPxqxrVaFwpASyeATbVwaB
-----------end_max5_patcher-----------
</code></pre>



Local forward draft (By John)
```
var app = require('http').createServer(handler)
var sio = require('socket.io');
var io = sio(app);
var l = sio(8081)
var fs = require('fs');

app.listen(8080);

function handler (req, res) {
    fs.readFile(__dirname + '/html.html',
                function (err, data) {
                    if (err) {
                        res.writeHead(500);
                        return res.end('Error loading html.html');
                    }

                    res.writeHead(200);
                    res.end(data);
                });
}

io.on('connection', function (socket) {
    socket.emit('msg', { hello: 'world' });
    socket.on('foo', function (data) {
        console.log('shithole');
    });
});


l.on('connection', function(socket) {
    socket.on('foo', function(data){
        console.log(data);
    });
});

and this in the browser:

<html>
  <head>
</head>
    <body>
      <script src="https://cdn.socket.io/socket.io-1.4.5.js"></script>
      <script>

        var socket = io.connect("http://live-stream.kinetecharts.org:8080")
        var socket2 = io.connect("localhost:8081");
        socket.on("msg", function(data){console.log(data); socket2.emit("foo", data);})
        //socket.on("msg", function(data){httpGetAsync("http://localhost:8080?msg", httpcb)});

        function httpcb(response){console.log(response)};
        </script>

</body>
</html>
```