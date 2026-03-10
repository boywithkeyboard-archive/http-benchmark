## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `121168` | `6943` | `130021` |
| **79%** | [Hyper Express](#hyper-express) | `96301` | `7005` | `100838` |
| **31%** | [Node (Default)](#node-default) | `37023` | `10370` | `102385` |
| **29%** | [Fastify](#fastify) | `34859` | `7512` | `46508` |
| **26%** | [Hono](#hono) | `31071` | `6603` | `44681` |
| **25%** | [Koa](#koa) | `29924` | `13743` | `101769` |
| **10%** | [Carbon](#carbon) | `12116` | `2468` | `16241` |
| **7%** | [Express](#express) | `8595` | `1635` | `11569` |


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
    Reqs/sec     13346.33   10856.24  118850.55
    Latency        3.74ms     3.72ms   319.27ms
    HTTP codes:
      1xx - 0, 2xx - 88363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11637
    Throughput:     2.68MB/s
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
    Reqs/sec      9605.65    9166.22  103447.34
    Latency        5.20ms     3.37ms   304.91ms
    HTTP codes:
      1xx - 0, 2xx - 88199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11801
    Throughput:     2.43MB/s
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
    Reqs/sec     39808.46   19880.18  113741.42
    Latency        1.25ms     1.66ms   140.85ms
    HTTP codes:
      1xx - 0, 2xx - 76308, 3xx - 0, 4xx - 0, 5xx - 0
      others - 23692
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 23692
    Throughput:     6.90MB/s
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
    Reqs/sec     34951.11   14496.59  107178.07
    Latency        1.42ms     1.69ms   144.54ms
    HTTP codes:
      1xx - 0, 2xx - 90142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9858
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
    Reqs/sec     98920.68    6664.66  104482.14
    Latency      502.09us   202.08us     9.15ms
    HTTP codes:
      1xx - 0, 2xx - 92159, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7841
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7841
    Throughput:    12.95MB/s
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
    Reqs/sec     29846.22   12938.55  103008.92
    Latency        1.67ms     1.77ms   151.75ms
    HTTP codes:
      1xx - 0, 2xx - 90344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9656
    Throughput:     6.10MB/s
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
    Reqs/sec     37820.48   10527.79  100940.69
    Latency        1.32ms     1.46ms   122.42ms
    HTTP codes:
      1xx - 0, 2xx - 96013, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3987
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3987
    Throughput:     8.32MB/s
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
    Reqs/sec    122513.72    7367.44  140734.30
    Latency      405.81us   136.86us     7.79ms
    HTTP codes:
      1xx - 0, 2xx - 96766, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3234
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3234
    Throughput:    18.75MB/s
  ```


