# relative-url

convert a partial url into an absolute url from your current
location.

In the WebSocket api, it expects a fully qualified url,
but this is awkward because we want to develop on localhost,
and then deploy to a server with a proper domain,
with a different port.

We want to create a WebSocket at a path, and get whatever domain
we are on.

This module generalized [wsurl](https://www.npmjs.com/package/wsurl)

## api: rurl(url, location, protocolMap, defaultProtocol)

`url` is your partial url, it will be described below.
`location` should be your is `window.location` or the output
of `require('url').parse(absoluteUrl)`

if you want to change the protocol, protocol map is a object
mapping current to desired protocol.

for websockets, you'll probably want: `{http: 'ws', https: 'wss'}`

`defaultProtocol` is what to set the protocol to otherwise.
for example, `'ws'` if `defaultProtocol` is not provided
the output protocol will the the same as `location.protocol`

## examples

for the following location:
``` js
var location = {
  protocol: 'http',
  host: 'server.com',
  pathname '/foo'
}
```
and the above mentioned settings (suitable for websockets)
output will be as following.
```
/ => ws://server.com/
/bar => ws://server.com/bar
//client.com => ws://client.com/
//:9999 => ws://server.com:9999/
?q=search => ws://server.com/foo?q=search
```

note that // denotes the start of the host.
if you want to set the port without changing the domain,
you must still use // otherwise the url module will
think it's a path. I considered detecting that,
but then i saw [this comment](https://github.com/nodejs/node/blob/master/lib/url.js#L207-L220)
and got scared of learning too much about urls.

## License

MIT








