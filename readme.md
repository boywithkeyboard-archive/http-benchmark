## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75590` | `2643` | `80172` |
| **84%** | [Hyper Express](#hyper-express) | `63346` | `3719` | `67785` |
| **33%** | [Node (Default)](#node-default) | `25174` | `7142` | `57370` |
| **32%** | [Fastify](#fastify) | `24007` | `7658` | `34552` |
| **28%** | [Hono](#hono) | `20983` | `6103` | `30295` |
| **26%** | [Koa](#koa) | `19442` | `10560` | `79252` |
| **11%** | [Carbon](#carbon) | `8202` | `1421` | `10300` |
| **8%** | [Express](#express) | `6211` | `1008` | `8376` |


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
    Reqs/sec      9054.25    7161.01   79612.96
    Latency        5.52ms     4.46ms   383.12ms
    HTTP codes:
      1xx - 0, 2xx - 89074, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10926
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10926
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
    Reqs/sec      6951.74    6192.40   73245.23
    Latency        7.18ms     4.05ms   369.38ms
    HTTP codes:
      1xx - 0, 2xx - 88988, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11012
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11012
    Throughput:     1.77MB/s
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
    Reqs/sec     23567.61    6909.97   35378.27
    Latency        2.12ms     2.04ms   181.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     20229.93    5508.59   29114.73
    Latency        2.47ms     2.07ms   186.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     63472.38    3409.75   66234.99
    Latency      785.45us    68.33us     3.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     18995.97    8346.22   66421.11
    Latency        2.63ms     2.38ms   210.58ms
    HTTP codes:
      1xx - 0, 2xx - 91452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8548
    Throughput:     3.93MB/s
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
    Reqs/sec     24186.59    7508.22   62089.32
    Latency        2.06ms     2.01ms   173.02ms
    HTTP codes:
      1xx - 0, 2xx - 97286, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2714
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2714
    Throughput:     5.38MB/s
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
    Reqs/sec     74430.32    2425.64   77844.52
    Latency      668.71us   172.39us     8.39ms
    HTTP codes:
      1xx - 0, 2xx - 96418, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3582
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3582
    Throughput:    11.36MB/s
  ```


