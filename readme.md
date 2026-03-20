## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `67793` | `5532` | `79131` |
| **84%** | [Hyper Express](#hyper-express) | `57199` | `3054` | `60209` |
| **33%** | [Node (Default)](#node-default) | `22377` | `6291` | `60169` |
| **33%** | [Hono](#hono) | `22242` | `7055` | `31256` |
| **31%** | [Fastify](#fastify) | `20855` | `5365` | `37687` |
| **27%** | [Koa](#koa) | `18106` | `7474` | `57262` |
| **12%** | [Carbon](#carbon) | `7868` | `1460` | `10309` |
| **9%** | [Express](#express) | `6352` | `1202` | `8462` |


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
    Reqs/sec      8757.42    7186.96   73958.40
    Latency        5.70ms     4.38ms   380.84ms
    HTTP codes:
      1xx - 0, 2xx - 88589, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11411
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11411
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
    Reqs/sec      6213.24    1127.68    8350.54
    Latency        8.04ms     3.79ms   363.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     20286.76    4640.61   37180.01
    Latency        2.46ms     2.05ms   186.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     22553.11    7168.24   31550.80
    Latency        2.21ms     2.14ms   192.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.10MB/s
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
    Reqs/sec     57184.62    3473.31   63731.69
    Latency        0.87ms    84.20us     3.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.12MB/s
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
    Reqs/sec     17624.96    8117.85   64976.12
    Latency        2.83ms     2.44ms   214.58ms
    HTTP codes:
      1xx - 0, 2xx - 92878, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7122
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7122
    Throughput:     3.70MB/s
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
    Reqs/sec     21426.79    4902.79   55099.30
    Latency        2.33ms     1.98ms   170.72ms
    HTTP codes:
      1xx - 0, 2xx - 97668, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2332
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2332
    Throughput:     4.79MB/s
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
    Reqs/sec     69382.33    3431.95   77807.43
    Latency      718.16us   139.58us     7.46ms
    HTTP codes:
      1xx - 0, 2xx - 97242, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2758
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2758
    Throughput:    10.68MB/s
  ```


