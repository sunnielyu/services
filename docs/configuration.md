## Configuration

Default configuration: [sample-config.json](../sample-config.json)

### listen

Allows customization of the `port` and `hostname` options in [http server documentation](https://nodejs.org/api/http.html#http_server_listen_port_hostname_backlog_callback).

Default:
```json
{
    "listen": {
        "port": 8000,
        "url": "http://localhost"
    }
}
```

### restApiRoot

The `restApiRoot` is the mountpoint the HTTP APIs will be set on. Default: '/'.

Example:
```json
"restApiRoot": "/_api"
```

### Socket

#### connections

The `connections` option can be used to establish P2P socket connections on start up. It defaults to an empty array.

Example:
```json
"socket": {
    "connections": [
        "http://domain1.com/users",
        "http://domain2.com/kittens"
    ]
}
```

After establishing a connection, events can be broadcast to the connections using the `io` instance exposed by `LabShare Services`.

### bodyParser

The `bodyParser` property allows options to be passed to the Express.js bodyParser.

Example:
```json
"bodyParser": {
  "json": {},
  "urlencoded": {
    "extended": true
  }
}
```

### Security

The configuration options in `security` allow you to add or modify `LabShare Service`'s default security headers, CORS settings, session cookies, and so on.

#### sessionOptions

The `sessionOptions` object corresponds to the options used by [express-session](https://www.npmjs.com/package/express-session#options).

Default:
```json
"security": {
    "sessionOptions": {
        "secret": "",
        "cookie": {
          "httpOnly": true,
          "maxAge": 3600000,
          "secure": false
        },
        "store": null,
        "storeOptions": {}
    }
}
```

By default, the sessions will use the `MemoryStore` from `express-session`. To use `connect-redis` instead, specify `store` as
`connect-redis` and pass options to `connect-redis` using `sessionOptions.storeOptions`.

Example:
```json
"security": {
    "sessionOptions": {
        "store": "connect-redis",
        "storeOptions": {
            // https://github.com/tj/connect-redis#options
        }
    }
}
```

You can also pass a constructor in the `store` option from https://github.com/expressjs/session#compatible-session-stores.

#### Additional

The `security` object can also contain directives for several other security libraries. For a complete list, visit [helmet](https://www.npmjs.com/package/helmet#how-it-works) and [helmet docs](https://helmetjs.github.io/docs/).

Default:
```json
"security": {
    "contentSecurityPolicy": false,
    "hpkp": false,
    "referrerPolicy": {
        "policy": "no-referrer"
    }
}
```

#### Elastic Application Performance Monitoring (APM)

Configuration of the Elastic APM Node.js agent to connect to the APM server is controlled via environment variables. These variables are optional if Elastic APM is not needed.

| Environment Variable   | Value |
| ------------- | ------------- |
| ELASTIC_APM_SERVICE_NAME  | Default value is pulled from `name` field in package.json. Will need to be overridden for `@labshare` packages. |
| ELASTIC_APM_SERVER_URL  | Url of Elastic APM server.  |
| ELASTIC_APM_SECRET_TOKEN  | (Optional) Secret token for Elastic APM server. |