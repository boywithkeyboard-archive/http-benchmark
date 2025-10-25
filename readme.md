## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75102` | `4424` | `81988` |
| **84%** | [Hyper Express](#hyper-express) | `62897` | `3514` | `66868` |
| **31%** | [Node (Default)](#node-default) | `23646` | `6357` | `69082` |
| **28%** | [Fastify](#fastify) | `21105` | `5000` | `35543` |
| **25%** | [Hono](#hono) | `18564` | `4168` | `29999` |
| **23%** | [Koa](#koa) | `16928` | `8162` | `76751` |
| **11%** | [Carbon](#carbon) | `8351` | `1445` | `10600` |
| **8%** | [Express](#express) | `6274` | `996` | `8369` |


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
    Reqs/sec      9071.17    5758.29   65353.74
    Latency        5.49ms     4.30ms   370.57ms
    HTTP codes:
      1xx - 0, 2xx - 91662, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8338
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8338
    Throughput:     1.89MB/s
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
    Reqs/sec      6241.47    1016.67    8307.03
    Latency        8.01ms     3.55ms   344.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     21609.56    5534.68   36894.00
    Latency        2.31ms     2.01ms   179.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     18340.39    4136.34   30683.78
    Latency        2.72ms     2.03ms   176.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.14MB/s
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
    Reqs/sec     62519.79    3751.12   68729.91
    Latency      797.47us    78.34us     2.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     17294.24    9166.61   77011.80
    Latency        2.89ms     2.48ms   216.35ms
    HTTP codes:
      1xx - 0, 2xx - 89605, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10395
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10395
    Throughput:     3.50MB/s
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
    Reqs/sec     23150.45    6176.02   69044.88
    Latency        2.15ms     1.89ms   163.81ms
    HTTP codes:
      1xx - 0, 2xx - 97207, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2793
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2793
    Throughput:     5.16MB/s
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
    Reqs/sec     74025.47    2937.62   79123.33
    Latency      672.97us   153.22us     9.95ms
    HTTP codes:
      1xx - 0, 2xx - 97523, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2477
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2477
    Throughput:    11.43MB/s
  ```


