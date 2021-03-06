# Ports, Urls and Versioning

### Short Names

- `self` refers to the current vessel. Normally used in `vessels.self...`.

### Ports

The Signal K HTTP and WebSocket services SHOULD be found on the usual HTTP/S ports (80 or 443). The services SHOULD be
found on the same port, but may be configured for independent ports and MAY be configured for ports other than HTTP/S.

A Signal K server MAY offer Signal K over TCP or UDP, these services SHOULD be on port 55555[[1]](#fn_1).

If an alternate port is needed it SHOULD be an arbitrary high port in the range 49152&ndash;65535[[2]](#fn_2).

### URL Prefix

The Signal K applications start from the `/signalk` root. This provides some protection against name collisions with
other applications on the same server. Therefore the Signal K entry point will always be found by loading
`http(s)://«host»:«port»/signalk`.

### Versioning

The version(s) of the Signal K API that a server supports SHALL be available as a JSON object available at `/signalk`:

```json
{
    "endpoints": {
        "v1": {
            "version": "1.1.2",
            "signalk-http": "http://192.168.1.2/signalk/v1/api/",
            "signalk-ws": "ws://192.168.1.2:34567/signalk/v1/stream"
        },
        "v3": {
            "version": "3.0",
            "signalk-http": "signalk/v3/api/",
            "signalk-ws": "ws://192.168.1.2/signalk/v3/stream",
            "signalk-tcp": "tcp://192.168.1.2:34568"
        }

    },
    "server": {
        "id": "signalk-node-server",
        "version": "0.1.21"
    }
}
```

This response is defined by the `discovery.json` schema. In this example, the server supports two versions of
the specification: `1.1.2` and `3.0`. For each version, the server indicates which transport protocols it
supports and the URL that can be used to access that protocol's endpoint; in the example, the `1.1.2`
REST endpoint is located at `http://192.168.1.2/signalk/v1/api/`. Clients should use one of these published
endpoints based on the protocol version they wish to use.

The server must only return valid URLs and should use IANA standard protocol names such as `http`.
However, a server may support unofficial protocols and may return additional protocol names; for example,
the response above indicates the server supports a `signalk-tcp` stream over TCP at on port `34568`.

A server may return relative URIs that the client must resolve against the base of the original request.

A server may return information about itself in the `server` property. The id and version scheme is not
defined as part of the specification and there is no registry for id values.
