## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73022` | `5512` | `77760` |
| **86%** | [Hyper Express](#hyper-express) | `62573` | `3590` | `68494` |
| **37%** | [Node (Default)](#node-default) | `26879` | `8183` | `51551` |
| **31%** | [Fastify](#fastify) | `22578` | `5924` | `35764` |
| **28%** | [Hono](#hono) | `20664` | `5719` | `30198` |
| **25%** | [Koa](#koa) | `18011` | `6174` | `52536` |
| **12%** | [Carbon](#carbon) | `8431` | `1400` | `10502` |
| **9%** | [Express](#express) | `6504` | `1011` | `8555` |


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
    Reqs/sec      8842.55    5115.38   67127.94
    Latency        5.65ms     4.13ms   359.18ms
    HTTP codes:
      1xx - 0, 2xx - 92254, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7746
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7746
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
    Reqs/sec      6998.57    4621.64   51287.12
    Latency        7.13ms     3.84ms   345.89ms
    HTTP codes:
      1xx - 0, 2xx - 91434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8559
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 7
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
    Reqs/sec     23889.39    6911.26   35845.71
    Latency        2.09ms     1.96ms   179.97ms
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
    Reqs/sec     20620.09    5699.25   29842.06
    Latency        2.42ms     1.94ms   173.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62263.43    3037.52   70829.60
    Latency      800.69us    67.77us     3.03ms
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
    Reqs/sec     17990.92    6002.33   43922.79
    Latency        2.77ms     2.29ms   202.37ms
    HTTP codes:
      1xx - 0, 2xx - 94860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5140
    Throughput:     3.86MB/s
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
    Reqs/sec     26074.32    7585.15   45563.99
    Latency        1.91ms     1.89ms   163.03ms
    HTTP codes:
      1xx - 0, 2xx - 98322, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1678
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1678
    Throughput:     5.87MB/s
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
    Reqs/sec     74419.98    6872.79   84468.64
    Latency      668.78us   262.17us    19.49ms
    HTTP codes:
      1xx - 0, 2xx - 96564, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3436
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3436
    Throughput:    11.36MB/s
  ```


