## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76198` | `2766` | `84663` |
| **83%** | [Hyper Express](#hyper-express) | `63089` | `6519` | `82435` |
| **33%** | [Node (Default)](#node-default) | `25344` | `7680` | `57403` |
| **31%** | [Fastify](#fastify) | `23630` | `7108` | `35589` |
| **28%** | [Hono](#hono) | `21191` | `5853` | `30295` |
| **25%** | [Koa](#koa) | `19015` | `8408` | `75878` |
| **11%** | [Carbon](#carbon) | `8539` | `1517` | `10548` |
| **9%** | [Express](#express) | `6522` | `1086` | `8375` |


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
    Reqs/sec      9174.32    6971.77   73916.27
    Latency        5.44ms     4.21ms   363.33ms
    HTTP codes:
      1xx - 0, 2xx - 89557, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10443
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10443
    Throughput:     1.87MB/s
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
    Reqs/sec      7072.23    5926.24   74595.05
    Latency        7.06ms     3.82ms   351.65ms
    HTTP codes:
      1xx - 0, 2xx - 90176, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9824
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9824
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
    Reqs/sec     23263.13    6963.37   36619.88
    Latency        2.15ms     2.03ms   185.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.28MB/s
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
    Reqs/sec     20442.56    5455.10   29691.68
    Latency        2.44ms     2.01ms   181.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     63307.11    4009.18   67515.56
    Latency      787.59us    79.17us     4.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     18352.24    8160.71   75940.26
    Latency        2.72ms     2.35ms   209.22ms
    HTTP codes:
      1xx - 0, 2xx - 92679, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7321
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7321
    Throughput:     3.84MB/s
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
    Reqs/sec     26905.03    9021.41   69845.46
    Latency        1.85ms     2.00ms   172.64ms
    HTTP codes:
      1xx - 0, 2xx - 96625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3375
    Throughput:     5.95MB/s
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
    Reqs/sec     74691.60    2694.85   80193.06
    Latency      665.98us   165.07us     9.29ms
    HTTP codes:
      1xx - 0, 2xx - 96645, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3355
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3355
    Throughput:    11.43MB/s
  ```


