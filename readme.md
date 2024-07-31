## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72951` | `6103` | `83605` |
| **84%** | [Hyper Express](#hyper-express) | `60979` | `3733` | `66159` |
| **34%** | [Fastify](#fastify) | `24454` | `7406` | `35721` |
| **33%** | [Node (Default)](#node-default) | `24171` | `6953` | `38336` |
| **28%** | [Hono](#hono) | `20429` | `5650` | `29789` |
| **25%** | [Koa](#koa) | `17895` | `6131` | `48714` |
| **11%** | [Carbon](#carbon) | `8185` | `1410` | `10381` |
| **9%** | [Express](#express) | `6571` | `1063` | `8560` |


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
    Reqs/sec      8725.72    4793.24   60345.78
    Latency        5.71ms     4.04ms   349.02ms
    HTTP codes:
      1xx - 0, 2xx - 92558, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7442
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7442
    Throughput:     1.84MB/s
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
    Reqs/sec      6678.05    1090.25    9109.46
    Latency        7.48ms     3.54ms   337.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     24326.28    7453.14   35812.45
    Latency        2.05ms     1.99ms   179.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     19781.42    5354.29   29632.50
    Latency        2.52ms     1.97ms   181.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.47MB/s
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
    Reqs/sec     61724.21    3273.76   65338.26
    Latency      807.95us    66.59us     2.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     18425.07    6231.32   57818.38
    Latency        2.71ms     2.38ms   207.19ms
    HTTP codes:
      1xx - 0, 2xx - 95035, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4965
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4965
    Throughput:     3.96MB/s
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
    Reqs/sec     24560.79    7500.82   38674.37
    Latency        2.03ms     1.92ms   164.62ms
    HTTP codes:
      1xx - 0, 2xx - 98349, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1651
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1650
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     5.53MB/s
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
    Reqs/sec     74131.80    6859.89   82738.22
    Latency      672.96us   190.15us    11.85ms
    HTTP codes:
      1xx - 0, 2xx - 98395, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1605
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1587
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 18
    Throughput:    11.53MB/s
  ```


