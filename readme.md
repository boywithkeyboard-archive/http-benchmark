## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73010` | `4816` | `79344` |
| **83%** | [Hyper Express](#hyper-express) | `60356` | `3768` | `64322` |
| **35%** | [Node (Default)](#node-default) | `25512` | `7626` | `53531` |
| **32%** | [Fastify](#fastify) | `23502` | `6985` | `36008` |
| **28%** | [Hono](#hono) | `20092` | `4915` | `28877` |
| **25%** | [Koa](#koa) | `18330` | `6220` | `50950` |
| **11%** | [Carbon](#carbon) | `8396` | `1418` | `10247` |
| **9%** | [Express](#express) | `6413` | `1032` | `8336` |


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
    Reqs/sec      8326.02    4286.05   53695.03
    Latency        6.00ms     4.31ms   374.83ms
    HTTP codes:
      1xx - 0, 2xx - 93133, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6867
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6867
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
    Reqs/sec      6504.39    1093.62    8387.85
    Latency        7.68ms     3.74ms   358.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23696.89    7106.11   36311.55
    Latency        2.11ms     2.06ms   181.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     20699.46    5543.56   28782.91
    Latency        2.41ms     2.08ms   186.37ms
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
    Reqs/sec     59808.82    3630.15   63714.95
    Latency      833.89us    80.55us     3.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     18574.72    6971.39   56022.51
    Latency        2.69ms     2.44ms   213.09ms
    HTTP codes:
      1xx - 0, 2xx - 93594, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6406
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6406
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
    Reqs/sec     26115.46    8080.62   51589.96
    Latency        1.91ms     1.87ms   161.52ms
    HTTP codes:
      1xx - 0, 2xx - 97516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2484
    Throughput:     5.84MB/s
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
    Reqs/sec     71639.04    4214.11   78240.71
    Latency      694.77us   186.55us     9.91ms
    HTTP codes:
      1xx - 0, 2xx - 96693, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3307
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3307
    Throughput:    10.96MB/s
  ```


