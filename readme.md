## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `124071` | `4502` | `130829` |
| **80%** | [Hyper Express](#hyper-express) | `99470` | `6866` | `107726` |
| **31%** | [Node (Default)](#node-default) | `38457` | `10587` | `98567` |
| **29%** | [Fastify](#fastify) | `35999` | `8321` | `55949` |
| **27%** | [Hono](#hono) | `33396` | `8373` | `48261` |
| **25%** | [Koa](#koa) | `31251` | `14208` | `111441` |
| **10%** | [Carbon](#carbon) | `11957` | `2365` | `16727` |
| **7%** | [Express](#express) | `8790` | `1791` | `12007` |


### In Detail

- #### Carbon
  [NPM](https://npmjs.com/@sinclair/carbon) | [GitHub](https://github.com/sinclairzx81/carbon)
  ```js
  import { listen } from '@sinclair/carbon/http'

  listen({
    hostname: '127.0.0.1',
    port: 3000
  }, () => {
    return new Response('Hello World', {
      status: 200,
      headers: {
        'content-type': 'text/plain'
      }
    })
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     13330.38   10850.37  119633.19
    Latency        3.73ms     3.76ms   324.73ms
    HTTP codes:
      1xx - 0, 2xx - 88868, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11132
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11132
    Throughput:     2.70MB/s
  ```

- #### Express
  [NPM](https://npmjs.com/express) | [GitHub](https://github.com/expressjs/express)
  ```js
  import express from 'express'

  const app = express()

  app.get('/', function (req, res) {
    res.send('Hello World')
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      9758.13   10395.18  114643.09
    Latency        5.11ms     3.29ms   299.85ms
    HTTP codes:
      1xx - 0, 2xx - 86628, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13372
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13372
    Throughput:     2.42MB/s
  ```

- #### Fastify
  [NPM](https://npmjs.com/fastify) | [GitHub](https://github.com/fastify/fastify)
  ```js
  import fastify from 'fastify'

  const app = fastify({
    logger: false
  })

  app.get('/', (req, res) => {
    res.send('Hello World')
  })

  app.listen({ port: 3000 }, (err) => {
    if (err) throw err
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     34889.26    7917.55   48596.04
    Latency        1.43ms     1.52ms   135.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.91MB/s
  ```

- #### Hono
  [NPM](https://npmjs.com/hono) | [GitHub](https://github.com/honojs/hono)
  ```js
  import { serve } from '@hono/node-server'
  import { Hono } from 'hono'

  const app = new Hono()

  app.get('/', (c) => c.text('Hello World'))

  serve(app)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     31536.67    7008.40   49174.86
    Latency        1.58ms     1.50ms   129.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.13MB/s
  ```

- #### Hyper Express
  [NPM](https://npmjs.com/hyper-express) | [GitHub](https://github.com/kartikk221/hyper-express)
  ```js
  import HyperExpress from 'hyper-express'

  const server = new HyperExpress.Server()

  server.get('/', (req, res) => {
    res.send('Hello World')
  })

  server.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec    100175.28    7195.71  106486.24
    Latency      497.34us    52.11us     2.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.23MB/s
  ```

- #### Koa
  [NPM](https://npmjs.com/koa) | [GitHub](https://github.com/koajs/koa)
  ```js
  import Koa from 'koa'

  const app = new Koa()

  app.use(ctx => {
    ctx.body = 'Hello World'
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     33479.06   17402.83  120484.44
    Latency        1.49ms     1.86ms   157.69ms
    HTTP codes:
      1xx - 0, 2xx - 87120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12880
    Throughput:     6.61MB/s
  ```

- #### Node (Default)
  [Website](https://nodejs.org/api/http.html)
  ```js
  import { createServer } from 'node:http'

  const server = createServer((req, res) => {
    res.writeHead(200, {
      'content-type': 'text/plain'
    })

    res.write('Hello World')

    res.end()
  })

  server.listen(3000, '127.0.0.1')
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     39504.19   13903.30  116782.25
    Latency        1.26ms     1.37ms   116.18ms
    HTTP codes:
      1xx - 0, 2xx - 93469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6531
    Throughput:     8.46MB/s
  ```

- #### uWS
  [GitHub](https://github.com/uNetworking/uWebSockets.js)
  ```js
  import { App } from 'uWebSockets.js'

  const app = App()

  app.get('/', (res, req) => {
    res.end('Hello World')
  })

  app.listen(3000, () => {})
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec    123559.18    5996.67  135079.29
    Latency      402.24us   147.86us     6.61ms
    HTTP codes:
      1xx - 0, 2xx - 96011, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3989
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3989
    Throughput:    18.78MB/s
  ```


