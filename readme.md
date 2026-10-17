## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72353` | `4143` | `85729` |
| **85%** | [Hyper Express](#hyper-express) | `61819` | `3142` | `66587` |
| **31%** | [Node (Default)](#node-default) | `22438` | `6079` | `59386` |
| **31%** | [Hono](#hono) | `22073` | `6419` | `32085` |
| **30%** | [Fastify](#fastify) | `22063` | `5430` | `38366` |
| **29%** | [Koa](#koa) | `20736` | `9484` | `71039` |
| **11%** | [Carbon](#carbon) | `7802` | `1308` | `10788` |
| **9%** | [Express](#express) | `6539` | `1106` | `8566` |


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
    Reqs/sec      8717.13    6621.27   77542.22
    Latency        5.73ms     4.43ms   374.16ms
    HTTP codes:
      1xx - 0, 2xx - 90213, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9787
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9787
    Throughput:     1.79MB/s
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
    Reqs/sec      6701.40    1176.45    9006.15
    Latency        7.46ms     3.72ms   356.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.92MB/s
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
    Reqs/sec     22760.80    5698.68   37383.41
    Latency        2.20ms     1.93ms   172.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.15MB/s
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
    Reqs/sec     22354.70    6528.16   30667.54
    Latency        2.23ms     2.22ms   195.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.05MB/s
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
    Reqs/sec     61459.33    3644.66   68597.86
    Latency      811.22us   100.23us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     19378.74    8963.88   78456.57
    Latency        2.57ms     2.38ms   208.02ms
    HTTP codes:
      1xx - 0, 2xx - 91653, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8347
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8347
    Throughput:     4.02MB/s
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
    Reqs/sec     22859.26    6757.15   76301.46
    Latency        2.18ms     2.01ms   163.60ms
    HTTP codes:
      1xx - 0, 2xx - 96169, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3831
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3831
    Throughput:     5.04MB/s
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
    Reqs/sec     72590.04    4611.21   82372.40
    Latency      685.89us   194.74us     8.45ms
    HTTP codes:
      1xx - 0, 2xx - 96716, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3284
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3284
    Throughput:    11.11MB/s
  ```


