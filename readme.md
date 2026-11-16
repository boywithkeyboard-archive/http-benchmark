## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `66476` | `7433` | `86091` |
| **85%** | [Hyper Express](#hyper-express) | `56702` | `3342` | `64168` |
| **37%** | [Node (Default)](#node-default) | `24402` | `8031` | `58932` |
| **34%** | [Fastify](#fastify) | `22524` | `7059` | `33795` |
| **28%** | [Hono](#hono) | `18283` | `5681` | `28090` |
| **26%** | [Koa](#koa) | `17090` | `9736` | `78097` |
| **12%** | [Carbon](#carbon) | `7679` | `1385` | `10206` |
| **9%** | [Express](#express) | `6151` | `1037` | `8104` |


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
    Reqs/sec      8578.40    5432.68   57378.94
    Latency        5.82ms     4.67ms   393.44ms
    HTTP codes:
      1xx - 0, 2xx - 89965, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10035
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10035
    Throughput:     1.75MB/s
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
    Reqs/sec      6188.46    1041.55    8202.63
    Latency        8.07ms     3.75ms   360.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     23624.37    7554.64   35545.72
    Latency        2.11ms     2.12ms   191.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.36MB/s
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
    Reqs/sec     18831.84    6047.14   27947.97
    Latency        2.65ms     2.05ms   184.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.25MB/s
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
    Reqs/sec     56632.61    3082.16   62967.12
    Latency        0.88ms    95.86us     3.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.04MB/s
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
    Reqs/sec     18211.04    8002.98   69084.23
    Latency        2.74ms     2.47ms   217.64ms
    HTTP codes:
      1xx - 0, 2xx - 92514, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7486
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7486
    Throughput:     3.81MB/s
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
    Reqs/sec     25336.04    8753.69   69395.82
    Latency        1.97ms     1.96ms   170.50ms
    HTTP codes:
      1xx - 0, 2xx - 96493, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3507
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3507
    Throughput:     5.59MB/s
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
    Reqs/sec     71225.75    5856.80   84252.42
    Latency      696.89us   238.19us    10.86ms
    HTTP codes:
      1xx - 0, 2xx - 96343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3657
    Throughput:    10.87MB/s
  ```


