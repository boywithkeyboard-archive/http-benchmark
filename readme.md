## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `67945` | `5556` | `80536` |
| **85%** | [Hyper Express](#hyper-express) | `57656` | `3468` | `63149` |
| **32%** | [Hono](#hono) | `21784` | `6717` | `29752` |
| **30%** | [Node (Default)](#node-default) | `20127` | `5662` | `71060` |
| **29%** | [Fastify](#fastify) | `19704` | `5101` | `35228` |
| **26%** | [Koa](#koa) | `17813` | `8193` | `73177` |
| **11%** | [Carbon](#carbon) | `7590` | `1429` | `10396` |
| **9%** | [Express](#express) | `6141` | `1108` | `8249` |


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
    Reqs/sec      8059.57    5451.72   62307.79
    Latency        6.19ms     4.69ms   394.26ms
    HTTP codes:
      1xx - 0, 2xx - 91892, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8108
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8108
    Throughput:     1.68MB/s
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
    Reqs/sec      6109.56    1038.23    8216.81
    Latency        8.18ms     3.77ms   362.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20209.02    4417.07   35559.58
    Latency        2.47ms     1.96ms   178.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     20474.42    6580.65   31014.83
    Latency        2.44ms     2.32ms   205.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     57461.09    3345.62   63708.13
    Latency        0.87ms    96.36us     4.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.16MB/s
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
    Reqs/sec     18823.67    8342.07   66485.18
    Latency        2.65ms     2.46ms   211.75ms
    HTTP codes:
      1xx - 0, 2xx - 93017, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6983
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6983
    Throughput:     3.96MB/s
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
    Reqs/sec     20061.49    5752.56   72651.08
    Latency        2.49ms     1.95ms   171.00ms
    HTTP codes:
      1xx - 0, 2xx - 96157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3843
    Throughput:     4.42MB/s
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
    Reqs/sec     69844.51    4435.56   79442.44
    Latency      713.22us   185.22us    12.75ms
    HTTP codes:
      1xx - 0, 2xx - 97003, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2997
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2997
    Throughput:    10.72MB/s
  ```


