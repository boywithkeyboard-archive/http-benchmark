## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74374` | `4676` | `84985` |
| **84%** | [Hyper Express](#hyper-express) | `62182` | `3852` | `65467` |
| **34%** | [Node (Default)](#node-default) | `25242` | `7414` | `52588` |
| **32%** | [Fastify](#fastify) | `23838` | `7468` | `35935` |
| **27%** | [Hono](#hono) | `20291` | `5545` | `29813` |
| **25%** | [Koa](#koa) | `18894` | `6446` | `54039` |
| **11%** | [Carbon](#carbon) | `8286` | `1383` | `10560` |
| **9%** | [Express](#express) | `6516` | `1031` | `8431` |


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
    Reqs/sec      8571.97    3963.89   51276.35
    Latency        5.82ms     4.13ms   356.09ms
    HTTP codes:
      1xx - 0, 2xx - 94406, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5594
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5594
    Throughput:     1.84MB/s
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
    Reqs/sec      6624.04    1084.34    8490.07
    Latency        7.54ms     3.53ms   337.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     23886.46    7195.38   35962.01
    Latency        2.09ms     1.94ms   177.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20207.01    5317.73   29316.83
    Latency        2.47ms     1.92ms   174.62ms
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
    Reqs/sec     62575.96    4229.28   72052.25
    Latency      796.62us    88.06us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18618.14    7549.41   57954.87
    Latency        2.68ms     2.45ms   211.48ms
    HTTP codes:
      1xx - 0, 2xx - 91207, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8793
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8793
    Throughput:     3.84MB/s
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
    Reqs/sec     25556.85    7529.85   62250.73
    Latency        1.95ms     1.88ms   161.84ms
    HTTP codes:
      1xx - 0, 2xx - 97457, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2543
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2539
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 4
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
    Reqs/sec     73944.96    7245.17  113113.24
    Latency      678.45us   177.56us    14.40ms
    HTTP codes:
      1xx - 0, 2xx - 96907, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3093
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3093
    Throughput:    11.25MB/s
  ```


