## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73919` | `4287` | `82508` |
| **84%** | [Hyper Express](#hyper-express) | `62122` | `3558` | `66189` |
| **34%** | [Node (Default)](#node-default) | `25232` | `8593` | `74624` |
| **32%** | [Fastify](#fastify) | `23543` | `7140` | `35675` |
| **29%** | [Hono](#hono) | `21140` | `6027` | `30507` |
| **25%** | [Koa](#koa) | `18551` | `8371` | `67524` |
| **11%** | [Carbon](#carbon) | `8183` | `1437` | `10424` |
| **9%** | [Express](#express) | `6411` | `1089` | `8370` |


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
    Reqs/sec      9186.83    6563.96   78080.60
    Latency        5.44ms     4.29ms   369.87ms
    HTTP codes:
      1xx - 0, 2xx - 90410, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9590
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9590
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
    Reqs/sec      7051.29    6339.85   79022.03
    Latency        7.08ms     3.97ms   364.48ms
    HTTP codes:
      1xx - 0, 2xx - 88933, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11067
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11067
    Throughput:     1.80MB/s
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
    Reqs/sec     24173.79    7654.76   36400.43
    Latency        2.07ms     2.03ms   177.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     21994.38    6244.22   30599.94
    Latency        2.27ms     2.01ms   181.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.97MB/s
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
    Reqs/sec     62103.33    3613.97   64664.53
    Latency      803.02us    76.05us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18452.80    7843.59   67493.35
    Latency        2.71ms     2.38ms   207.60ms
    HTTP codes:
      1xx - 0, 2xx - 93184, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6816
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6816
    Throughput:     3.89MB/s
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
    Reqs/sec     24880.83    7772.93   62961.23
    Latency        2.01ms     1.88ms   164.48ms
    HTTP codes:
      1xx - 0, 2xx - 97407, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2593
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2593
    Throughput:     5.55MB/s
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
    Reqs/sec     73967.45    2451.28   80085.03
    Latency      672.79us   186.81us    10.46ms
    HTTP codes:
      1xx - 0, 2xx - 96449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3551
    Throughput:    11.29MB/s
  ```


