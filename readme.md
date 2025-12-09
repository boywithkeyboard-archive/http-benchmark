## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74127` | `3693` | `93122` |
| **84%** | [Hyper Express](#hyper-express) | `62276` | `3562` | `67041` |
| **35%** | [Node (Default)](#node-default) | `25791` | `8327` | `65846` |
| **31%** | [Fastify](#fastify) | `22884` | `6996` | `34858` |
| **27%** | [Koa](#koa) | `19830` | `11215` | `83600` |
| **27%** | [Hono](#hono) | `19772` | `5759` | `29128` |
| **11%** | [Carbon](#carbon) | `8268` | `1445` | `10379` |
| **9%** | [Express](#express) | `6444` | `1099` | `8299` |


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
    Reqs/sec      8745.58    5117.96   62155.83
    Latency        5.71ms     4.39ms   374.62ms
    HTTP codes:
      1xx - 0, 2xx - 92306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7694
    Throughput:     1.83MB/s
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
    Reqs/sec      6496.12    1101.10    8311.55
    Latency        7.69ms     3.72ms   355.88ms
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
    Reqs/sec     23211.24    6715.91   34701.40
    Latency        2.15ms     2.09ms   189.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.27MB/s
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
    Reqs/sec     21208.15    5800.41   30782.15
    Latency        2.36ms     2.06ms   186.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62224.53    2942.45   64374.25
    Latency      800.98us    67.75us     2.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18840.77    9885.58   79586.07
    Latency        2.65ms     2.47ms   214.55ms
    HTTP codes:
      1xx - 0, 2xx - 89580, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10420
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10420
    Throughput:     3.82MB/s
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
    Reqs/sec     25576.20    8118.33   63779.11
    Latency        1.95ms     1.96ms   168.65ms
    HTTP codes:
      1xx - 0, 2xx - 97409, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2591
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2591
    Throughput:     5.71MB/s
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
    Reqs/sec     74824.75    1960.91   84493.07
    Latency      665.32us   162.26us     8.59ms
    HTTP codes:
      1xx - 0, 2xx - 93900, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6100
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6100
    Throughput:    11.12MB/s
  ```


