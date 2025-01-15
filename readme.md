## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73663` | `5071` | `81740` |
| **82%** | [Hyper Express](#hyper-express) | `60693` | `3954` | `64235` |
| **35%** | [Node (Default)](#node-default) | `25968` | `7650` | `56426` |
| **32%** | [Fastify](#fastify) | `23382` | `7010` | `36472` |
| **29%** | [Hono](#hono) | `21343` | `6157` | `29599` |
| **25%** | [Koa](#koa) | `18623` | `6740` | `55394` |
| **11%** | [Carbon](#carbon) | `8203` | `1499` | `10515` |
| **8%** | [Express](#express) | `6247` | `1036` | `8290` |


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
    Reqs/sec      8636.30    4491.90   53581.69
    Latency        5.78ms     4.33ms   373.65ms
    HTTP codes:
      1xx - 0, 2xx - 93469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6531
    Throughput:     1.83MB/s
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
    Reqs/sec      6250.32    1007.96    8274.73
    Latency        8.00ms     3.75ms   360.77ms
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
    Reqs/sec     24543.09    7712.28   36290.83
    Latency        2.04ms     2.15ms   193.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     21205.47    6791.36   30817.80
    Latency        2.36ms     2.24ms   193.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     61296.36    3264.54   64860.49
    Latency      812.75us    67.19us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     18461.60    6276.84   48923.86
    Latency        2.70ms     2.47ms   214.07ms
    HTTP codes:
      1xx - 0, 2xx - 93673, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6327
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6327
    Throughput:     3.91MB/s
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
    Reqs/sec     24989.35    6797.38   49380.09
    Latency        2.00ms     1.99ms   172.34ms
    HTTP codes:
      1xx - 0, 2xx - 97857, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2143
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2143
    Throughput:     5.60MB/s
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
    Reqs/sec     74047.12    5652.74   81390.48
    Latency      672.42us   170.38us     8.16ms
    HTTP codes:
      1xx - 0, 2xx - 97411, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2589
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2589
    Throughput:    11.42MB/s
  ```


