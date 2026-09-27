## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68953` | `3942` | `79647` |
| **84%** | [Hyper Express](#hyper-express) | `57889` | `3018` | `62173` |
| **29%** | [Fastify](#fastify) | `20282` | `5623` | `35265` |
| **29%** | [Node (Default)](#node-default) | `20113` | `5716` | `71604` |
| **29%** | [Koa](#koa) | `19945` | `8989` | `70517` |
| **28%** | [Hono](#hono) | `19610` | `5782` | `28975` |
| **11%** | [Carbon](#carbon) | `7361` | `1234` | `10260` |
| **9%** | [Express](#express) | `5933` | `1047` | `8240` |


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
    Reqs/sec      8498.43    7893.33   73307.81
    Latency        5.87ms     4.61ms   393.18ms
    HTTP codes:
      1xx - 0, 2xx - 85838, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14162
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14162
    Throughput:     1.66MB/s
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
    Reqs/sec      6038.83    1049.66    8058.71
    Latency        8.28ms     3.87ms   369.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     20327.78    4931.85   35672.52
    Latency        2.46ms     2.04ms   186.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     21358.11    6460.57   30733.94
    Latency        2.34ms     2.28ms   197.80ms
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
    Reqs/sec     58545.35    3581.29   65313.81
    Latency        0.85ms   100.20us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.31MB/s
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
    Reqs/sec     18748.70    9417.62   73907.96
    Latency        2.66ms     2.54ms   220.44ms
    HTTP codes:
      1xx - 0, 2xx - 90167, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9833
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9833
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
    Reqs/sec     20346.91    5632.94   64412.64
    Latency        2.45ms     2.00ms   175.93ms
    HTTP codes:
      1xx - 0, 2xx - 96453, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3547
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3547
    Throughput:     4.49MB/s
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
    Reqs/sec     68876.84    4430.25   79352.26
    Latency      723.72us   159.35us     7.92ms
    HTTP codes:
      1xx - 0, 2xx - 97344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2656
    Throughput:    10.61MB/s
  ```


