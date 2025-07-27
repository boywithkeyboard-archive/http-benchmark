## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75272` | `2034` | `82079` |
| **83%** | [Hyper Express](#hyper-express) | `62337` | `3293` | `65598` |
| **36%** | [Node (Default)](#node-default) | `26779` | `8694` | `73070` |
| **32%** | [Fastify](#fastify) | `24279` | `7406` | `36350` |
| **28%** | [Hono](#hono) | `21407` | `5841` | `29489` |
| **27%** | [Koa](#koa) | `20063` | `7991` | `66174` |
| **11%** | [Carbon](#carbon) | `8182` | `1389` | `10397` |
| **8%** | [Express](#express) | `6380` | `1006` | `8289` |


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
    Reqs/sec      8927.18    5908.43   69900.10
    Latency        5.59ms     4.19ms   363.95ms
    HTTP codes:
      1xx - 0, 2xx - 91555, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8445
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8445
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
    Reqs/sec      7272.04    6170.40   77978.61
    Latency        6.87ms     3.91ms   360.18ms
    HTTP codes:
      1xx - 0, 2xx - 89930, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10070
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10070
    Throughput:     1.87MB/s
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
    Reqs/sec     25623.18    7948.45   36222.57
    Latency        1.95ms     1.94ms   174.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.81MB/s
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
    Reqs/sec     21016.19    5770.12   29475.94
    Latency        2.38ms     2.06ms   186.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63401.79    3515.66   65746.85
    Latency      786.57us    67.80us     3.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     19223.42    9072.20   77549.64
    Latency        2.59ms     2.36ms   209.97ms
    HTTP codes:
      1xx - 0, 2xx - 91478, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8522
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8522
    Throughput:     3.98MB/s
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
    Reqs/sec     25972.05    7969.06   64125.36
    Latency        1.92ms     1.87ms   163.15ms
    HTTP codes:
      1xx - 0, 2xx - 97026, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2974
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2974
    Throughput:     5.78MB/s
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
    Reqs/sec     75305.06    2476.08   79181.52
    Latency      660.47us   214.73us    13.97ms
    HTTP codes:
      1xx - 0, 2xx - 95453, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4547
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4547
    Throughput:    11.38MB/s
  ```


