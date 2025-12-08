## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75020` | `3266` | `84554` |
| **86%** | [Hyper Express](#hyper-express) | `64154` | `3813` | `66725` |
| **36%** | [Node (Default)](#node-default) | `26898` | `8678` | `74581` |
| **32%** | [Fastify](#fastify) | `24205` | `7265` | `36204` |
| **28%** | [Hono](#hono) | `21353` | `6367` | `31498` |
| **25%** | [Koa](#koa) | `18648` | `7986` | `69496` |
| **11%** | [Carbon](#carbon) | `8466` | `1472` | `10503` |
| **9%** | [Express](#express) | `6486` | `1104` | `8416` |


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
    Reqs/sec      9382.65    8027.40   81758.11
    Latency        5.32ms     4.28ms   365.36ms
    HTTP codes:
      1xx - 0, 2xx - 87624, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12376
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12376
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
    Reqs/sec      6951.41    5124.18   66424.63
    Latency        7.18ms     3.89ms   357.98ms
    HTTP codes:
      1xx - 0, 2xx - 91961, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8039
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8039
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
    Reqs/sec     23490.38    6425.74   35172.10
    Latency        2.13ms     1.96ms   176.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     20939.48    5921.96   29838.03
    Latency        2.38ms     2.01ms   177.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     63835.17    3096.22   66688.98
    Latency      781.43us    69.46us     3.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     18635.32    7481.08   63836.18
    Latency        2.68ms     2.33ms   205.96ms
    HTTP codes:
      1xx - 0, 2xx - 93494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6506
    Throughput:     3.94MB/s
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
    Reqs/sec     26120.33    7975.72   64016.43
    Latency        1.91ms     1.89ms   163.63ms
    HTTP codes:
      1xx - 0, 2xx - 97452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2548
    Throughput:     5.83MB/s
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
    Reqs/sec     75101.19    4872.34   86448.53
    Latency      661.55us   219.21us    13.49ms
    HTTP codes:
      1xx - 0, 2xx - 95631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4369
    Throughput:    11.37MB/s
  ```


