## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73065` | `7083` | `84042` |
| **86%** | [Hyper Express](#hyper-express) | `62691` | `3092` | `67526` |
| **36%** | [Node (Default)](#node-default) | `26162` | `8018` | `58169` |
| **32%** | [Fastify](#fastify) | `23620` | `7081` | `36632` |
| **29%** | [Hono](#hono) | `20923` | `5725` | `30354` |
| **24%** | [Koa](#koa) | `17625` | `6065` | `48053` |
| **11%** | [Carbon](#carbon) | `8240` | `1372` | `10742` |
| **9%** | [Express](#express) | `6603` | `1074` | `8650` |


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
    Reqs/sec      8729.15    3798.92   51748.11
    Latency        5.72ms     4.10ms   355.68ms
    HTTP codes:
      1xx - 0, 2xx - 94891, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5109
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5109
    Throughput:     1.88MB/s
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
    Reqs/sec      6570.90    1046.15    8522.43
    Latency        7.61ms     3.56ms   342.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     25065.61    7565.88   36017.51
    Latency        1.99ms     1.82ms   167.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
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
    Reqs/sec     20710.07    5908.22   30606.29
    Latency        2.41ms     1.91ms   174.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62964.08    3142.63   66377.00
    Latency      792.22us    74.06us     4.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     17835.38    6183.32   52858.01
    Latency        2.80ms     2.35ms   204.32ms
    HTTP codes:
      1xx - 0, 2xx - 94300, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5700
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5700
    Throughput:     3.80MB/s
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
    Reqs/sec     26208.41    8202.76   41543.91
    Latency        1.90ms     1.93ms   165.68ms
    HTTP codes:
      1xx - 0, 2xx - 98379, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1621
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1621
    Throughput:     5.90MB/s
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
    Reqs/sec     73221.75    5364.23   88977.58
    Latency      680.30us   247.33us    15.31ms
    HTTP codes:
      1xx - 0, 2xx - 96074, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3926
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3926
    Throughput:    11.12MB/s
  ```


