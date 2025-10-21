## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75446` | `3596` | `83627` |
| **82%** | [Hyper Express](#hyper-express) | `61873` | `3537` | `66066` |
| **34%** | [Node (Default)](#node-default) | `25885` | `8606` | `77511` |
| **30%** | [Fastify](#fastify) | `22651` | `6814` | `34901` |
| **28%** | [Hono](#hono) | `20991` | `6276` | `30510` |
| **26%** | [Koa](#koa) | `19873` | `11460` | `81196` |
| **11%** | [Carbon](#carbon) | `8065` | `1428` | `10187` |
| **8%** | [Express](#express) | `6365` | `1088` | `8300` |


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
    Reqs/sec      8684.78    6010.59   76762.73
    Latency        5.74ms     4.46ms   389.09ms
    HTTP codes:
      1xx - 0, 2xx - 91390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8610
    Throughput:     1.80MB/s
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
    Reqs/sec      6340.94    1061.28    8254.64
    Latency        7.88ms     3.74ms   361.60ms
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
    Reqs/sec     24092.23    7527.24   35063.28
    Latency        2.07ms     2.12ms   187.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20198.46    5269.66   28704.21
    Latency        2.47ms     2.05ms   186.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     61972.26    4672.85   64660.84
    Latency      804.68us   104.02us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18891.62    7755.48   64324.48
    Latency        2.64ms     2.43ms   212.06ms
    HTTP codes:
      1xx - 0, 2xx - 93083, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6917
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6917
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
    Reqs/sec     25233.60    8181.96   63644.17
    Latency        1.98ms     1.98ms   171.22ms
    HTTP codes:
      1xx - 0, 2xx - 97528, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2472
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2472
    Throughput:     5.63MB/s
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
    Reqs/sec     74821.04    3138.11   94568.43
    Latency      667.72us   198.19us     9.91ms
    HTTP codes:
      1xx - 0, 2xx - 96332, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3668
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3668
    Throughput:    11.36MB/s
  ```


