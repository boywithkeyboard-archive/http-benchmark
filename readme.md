## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73636` | `3568` | `83569` |
| **84%** | [Hyper Express](#hyper-express) | `62137` | `3604` | `69280` |
| **33%** | [Node (Default)](#node-default) | `24204` | `7093` | `69760` |
| **31%** | [Fastify](#fastify) | `22985` | `6606` | `34523` |
| **29%** | [Hono](#hono) | `21162` | `6373` | `30454` |
| **24%** | [Koa](#koa) | `17880` | `8399` | `76787` |
| **11%** | [Carbon](#carbon) | `8122` | `1453` | `10345` |
| **8%** | [Express](#express) | `6251` | `1071` | `8252` |


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
    Reqs/sec      8652.32    6979.01   79744.87
    Latency        5.77ms     4.39ms   374.92ms
    HTTP codes:
      1xx - 0, 2xx - 89452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10548
    Throughput:     1.76MB/s
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
    Reqs/sec      6386.30    1101.59    8298.16
    Latency        7.83ms     3.77ms   362.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23632.25    7316.22   35103.96
    Latency        2.11ms     2.08ms   184.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.36MB/s
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
    Reqs/sec     21641.51    6256.50   30805.06
    Latency        2.31ms     2.04ms   185.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     62714.97    3954.66   70434.89
    Latency      795.10us    74.83us     2.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     18211.07    8047.25   70478.26
    Latency        2.74ms     2.44ms   217.66ms
    HTTP codes:
      1xx - 0, 2xx - 92713, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7287
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7287
    Throughput:     3.82MB/s
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
    Reqs/sec     24230.34    7165.49   59461.76
    Latency        2.06ms     2.03ms   173.31ms
    HTTP codes:
      1xx - 0, 2xx - 97514, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2486
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2486
    Throughput:     5.42MB/s
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
    Reqs/sec     74622.00    2668.41   81616.50
    Latency      667.92us   163.18us     7.65ms
    HTTP codes:
      1xx - 0, 2xx - 93694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6306
    Throughput:    11.05MB/s
  ```


