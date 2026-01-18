## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74164` | `2922` | `82080` |
| **85%** | [Hyper Express](#hyper-express) | `63306` | `3082` | `67061` |
| **35%** | [Node (Default)](#node-default) | `25790` | `7633` | `56094` |
| **31%** | [Fastify](#fastify) | `22960` | `6451` | `35378` |
| **28%** | [Hono](#hono) | `21112` | `5778` | `30361` |
| **24%** | [Koa](#koa) | `18091` | `7675` | `64347` |
| **11%** | [Carbon](#carbon) | `8193` | `1474` | `10310` |
| **9%** | [Express](#express) | `6352` | `1024` | `8234` |


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
    Reqs/sec      8797.79    5533.41   71614.38
    Latency        5.67ms     4.39ms   371.62ms
    HTTP codes:
      1xx - 0, 2xx - 92425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7575
    Throughput:     1.85MB/s
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
    Reqs/sec      6344.08    1015.92    8269.49
    Latency        7.88ms     3.70ms   354.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     23382.81    6864.25   35511.59
    Latency        2.14ms     1.95ms   174.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.31MB/s
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
    Reqs/sec     19986.42    5341.11   28964.32
    Latency        2.50ms     2.09ms   186.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     62462.51    3346.64   64909.63
    Latency      798.36us    70.13us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18738.40    8032.52   70918.29
    Latency        2.66ms     2.38ms   208.84ms
    HTTP codes:
      1xx - 0, 2xx - 92802, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7198
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7198
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
    Reqs/sec     27088.23    8557.60   70197.62
    Latency        1.84ms     1.92ms   162.43ms
    HTTP codes:
      1xx - 0, 2xx - 96663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3337
    Throughput:     5.99MB/s
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
    Reqs/sec     74950.18    2951.43   83725.96
    Latency      664.49us   184.62us     9.55ms
    HTTP codes:
      1xx - 0, 2xx - 96503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3497
    Throughput:    11.44MB/s
  ```


