## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74922` | `4316` | `101333` |
| **83%** | [Hyper Express](#hyper-express) | `62346` | `3802` | `67612` |
| **30%** | [Node (Default)](#node-default) | `22259` | `6099` | `63584` |
| **29%** | [Fastify](#fastify) | `21581` | `5129` | `35242` |
| **24%** | [Hono](#hono) | `18264` | `4053` | `29143` |
| **22%** | [Koa](#koa) | `16428` | `7023` | `66048` |
| **11%** | [Carbon](#carbon) | `8041` | `1459` | `10484` |
| **8%** | [Express](#express) | `6365` | `1100` | `8324` |


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
    Reqs/sec      8479.73    5586.24   69746.12
    Latency        5.88ms     4.37ms   375.38ms
    HTTP codes:
      1xx - 0, 2xx - 92149, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7851
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7851
    Throughput:     1.78MB/s
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
    Reqs/sec      6283.72    1061.42    8342.28
    Latency        7.95ms     3.82ms   365.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     21126.79    5058.17   34614.49
    Latency        2.37ms     2.11ms   189.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     18728.33    4979.66   30171.94
    Latency        2.67ms     2.12ms   189.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.23MB/s
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
    Reqs/sec     62708.04    3792.92   67179.98
    Latency      795.34us    68.81us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     16342.02    7202.16   71899.32
    Latency        3.05ms     2.45ms   217.06ms
    HTTP codes:
      1xx - 0, 2xx - 92263, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7737
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7737
    Throughput:     3.41MB/s
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
    Reqs/sec     23392.93    7148.83   76030.80
    Latency        2.13ms     2.00ms   169.14ms
    HTTP codes:
      1xx - 0, 2xx - 96487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3513
    Throughput:     5.17MB/s
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
    Reqs/sec     74872.82    3530.09   98753.53
    Latency      667.56us   183.72us     9.70ms
    HTTP codes:
      1xx - 0, 2xx - 96328, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3672
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3672
    Throughput:    11.36MB/s
  ```


