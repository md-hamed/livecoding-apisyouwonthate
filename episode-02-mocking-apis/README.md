[Video](https://youtu.be/xRJovXtzLPc)

Using a mock server, you can basically see how an API would work, without actually
building anything.

Apiary.io will give you a mock server without you needing to do anything.

Go to your API in Apiary and click Inspector. At the top it will have a mock server host name:

http://private-3a141-philsturgeon.apiary-mock.com/

If you navigate to this in your browser, you will see a list of routes defined in your Apiary, so
grab one of those.

``` shell
$ http GET http://private-3a141-philsturgeon.apiary-mock.com/products
```

Here you will see output, generated from our data structures and example values:

``` http
HTTP/1.1 200 OK
Access-Control-Allow-Methods: OPTIONS,GET,HEAD,POST,PUT,DELETE,TRACE,CONNECT
Access-Control-Allow-Origin: *
Access-Control-Max-Age: 10
Connection: keep-alive
Content-Length: 320
Content-Type: application/json
Date: Tue, 30 Aug 2016 18:49:49 GMT
Server: Cowboy
Via: 1.1 vegur
X-Apiary-Ratelimit-Limit: 120
X-Apiary-Ratelimit-Remaining: 119
X-Apiary-Transaction-Id: 57c5d54d0ace330b004cddee

[
    {
        "apv": 7.4,
        "description": "Tastes like apple juice but knocks you on your arse.",
        "id": "djkfh34t234t",
        "image_url": "http://example.com/ciders/old-bris.jpg",
        "name": "Old Bristolian",
        "relationships": {
            "manufacturer": "",
            "recent_reviews": ""
        },
        "type": "cider"
    }
]
```

Alternatively, because Apiary is a paid service, you might want to use some free tools. Running
a mock server locally is easy with drakov!

``` shell
$ npm install -g drakov
$ drakov -f apiary.apib
```

Again, you can use a HTTP client to test the response:

```
$ http GET http://localhost:3000/products --json
```

``` http
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 344
Content-Type: application/json
Date: Tue, 30 Aug 2016 19:03:33 GMT
ETag: W/"158-JzY5bB7n307mn5JL95pPGw"
X-Powered-By: Drakov API Server

[
    {
        "apv": 7.4,
        "description": "Tastes like apple juice but knocks you on your arse.",
        "id": "d66c7429-d8e0-4772-85b4-c18e66564e88",
        "image_url": "http://example.com/ciders/old-bris.jpg",
        "name": "Old Bristolian",
        "relationships": {
            "manufacturer": "",
            "recent_reviews": ""
        },
        "type": "cider"
    }
]

Then, if you want to share this with other people, one solution is to offer your local copy over [ngrok](https://ngrok.com/). This will
make a local tunnel to the drakov server you have running.

``` shell
$ brew cask install ngrok
```

Assuming you still have drakov running on port 3000, you can now run this in another session:

```
$ ngrok http 3000

ngrok by @inconshreveable                                                            (Ctrl+C to quit)

Tunnel Status                 online
Version                       2.1.3
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://xxxxxx.ngrok.io -> localhost:3000
Forwarding                    https://xxxxxx.ngrok.io -> localhost:3000
```

The Web Interface will give you an inspector very much like Apiary, and the forwarding links are a host you can share with others on your team.

# In ANOTHER tab...
$ http GET http://xxxxxx.ngrok.io/products --json
```

Once again you should see the collection. You can now share the `http://xxxxxx.ngrok.io` host name with your coworkers, family and friends, and
snoop on the traffic as it comes in, if you should so wish.

If you'd like to have a more permanent solution, consider shoving draokv on a free Heroku instance.

Personally, I quite like having the tunnel turn off in my control, as it stops people relying on this mock for too long. It gives them a chance to give feedback, but stops people relying on it for their test suites perminently.
