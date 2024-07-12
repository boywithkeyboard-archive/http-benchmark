## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76534` | `6651` | `81583` |
| **85%** | [Hyper Express](#hyper-express) | `64898` | `4241` | `83924` |
| **34%** | [Node (Default)](#node-default) | `25651` | `7589` | `40863` |
| **32%** | [Fastify](#fastify) | `24218` | `7426` | `35920` |
| **26%** | [Hono](#hono) | `20092` | `5603` | `29903` |
| **25%** | [Koa](#koa) | `19040` | `6330` | `49242` |
| **11%** | [Carbon](#carbon) | `8306` | `1428` | `10463` |
| **9%** | [Express](#express) | `6621` | `1098` | `8426` |


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
    Reqs/sec      8756.64    4481.83   61464.53
    Latency        5.70ms     4.19ms   366.12ms
    HTTP codes:
      1xx - 0, 2xx - 93695, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6305
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6305
    Throughput:     1.86MB/s
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
    Reqs/sec      6420.44     962.55    8343.44
    Latency        7.78ms     3.59ms   346.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     24721.88    8082.65   38258.33
    Latency        2.02ms     1.99ms   181.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21330.51    6221.65   29767.62
    Latency        2.34ms     1.89ms   170.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     64933.35    3801.70   72386.71
    Latency      768.84us    60.71us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.21MB/s
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
    Reqs/sec     18276.86    7307.70   67284.09
    Latency        2.73ms     2.37ms   208.02ms
    HTTP codes:
      1xx - 0, 2xx - 92015, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7985
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7985
    Throughput:     3.80MB/s
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
    Reqs/sec     25742.84    8099.75   51515.00
    Latency        1.94ms     1.93ms   169.82ms
    HTTP codes:
      1xx - 0, 2xx - 96956, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3044
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3034
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 10
    Throughput:     5.70MB/s
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
    Reqs/sec     76187.36    5867.73   87022.18
    Latency      654.40us   206.86us    10.66ms
    HTTP codes:
      1xx - 0, 2xx - 97506, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2494
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2493
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:    11.75MB/s
  ```


