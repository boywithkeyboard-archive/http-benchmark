## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74719` | `5394` | `85216` |
| **85%** | [Hyper Express](#hyper-express) | `63587` | `3706` | `67835` |
| **35%** | [Node (Default)](#node-default) | `26175` | `7723` | `40526` |
| **32%** | [Fastify](#fastify) | `24132` | `7008` | `35002` |
| **28%** | [Hono](#hono) | `21239` | `5276` | `29410` |
| **24%** | [Koa](#koa) | `18182` | `6539` | `53237` |
| **11%** | [Carbon](#carbon) | `8283` | `1375` | `10514` |
| **9%** | [Express](#express) | `6580` | `1019` | `8629` |


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
    Reqs/sec      8986.69    4564.52   50735.22
    Latency        5.55ms     4.15ms   358.38ms
    HTTP codes:
      1xx - 0, 2xx - 93382, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6618
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6618
    Throughput:     1.91MB/s
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
    Reqs/sec      7028.86    3650.96   48146.23
    Latency        7.10ms     3.86ms   350.44ms
    HTTP codes:
      1xx - 0, 2xx - 93646, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6354
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6312
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 42
    Throughput:     1.88MB/s
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
    Reqs/sec     24462.55    7284.03   36232.34
    Latency        2.04ms     1.91ms   172.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     21226.53    5488.64   29273.44
    Latency        2.35ms     1.95ms   174.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     64118.80    2929.47   68928.87
    Latency      777.38us    65.66us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     18747.06    6234.13   51197.13
    Latency        2.66ms     2.27ms   199.89ms
    HTTP codes:
      1xx - 0, 2xx - 94700, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5300
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5300
    Throughput:     4.01MB/s
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
    Reqs/sec     26238.57    7859.44   58011.68
    Latency        1.90ms     1.78ms   165.33ms
    HTTP codes:
      1xx - 0, 2xx - 97569, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2431
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2431
    Throughput:     5.85MB/s
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
    Reqs/sec     74171.40    6487.62   86567.23
    Latency      671.73us   188.58us    12.62ms
    HTTP codes:
      1xx - 0, 2xx - 97146, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2854
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2854
    Throughput:    11.40MB/s
  ```


