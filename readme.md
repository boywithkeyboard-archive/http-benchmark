## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76699` | `7006` | `83875` |
| **79%** | [Hyper Express](#hyper-express) | `60552` | `4151` | `68275` |
| **35%** | [Node (Default)](#node-default) | `26592` | `8089` | `41960` |
| **31%** | [Fastify](#fastify) | `23884` | `7519` | `35816` |
| **27%** | [Hono](#hono) | `20580` | `5881` | `29882` |
| **23%** | [Koa](#koa) | `17966` | `6686` | `52044` |
| **11%** | [Carbon](#carbon) | `8253` | `1441` | `10474` |
| **8%** | [Express](#express) | `6519` | `1039` | `8501` |


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
    Reqs/sec      8869.21    4831.80   62066.15
    Latency        5.63ms     4.46ms   372.36ms
    HTTP codes:
      1xx - 0, 2xx - 92393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7607
    Throughput:     1.86MB/s
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
    Reqs/sec      6363.10     990.60    8515.11
    Latency        7.85ms     3.53ms   343.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     24198.86    7210.00   35615.37
    Latency        2.06ms     1.95ms   177.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20640.45    5858.03   30379.74
    Latency        2.42ms     1.89ms   174.20ms
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
    Reqs/sec     62903.77    3662.42   66475.69
    Latency      793.11us    73.12us     3.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     18820.33    7327.86   60082.98
    Latency        2.65ms     2.27ms   199.40ms
    HTTP codes:
      1xx - 0, 2xx - 91831, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8169
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8169
    Throughput:     3.91MB/s
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
    Reqs/sec     26326.03    8388.72   64490.64
    Latency        1.89ms     1.88ms   165.95ms
    HTTP codes:
      1xx - 0, 2xx - 96442, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3558
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3552
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 6
    Throughput:     5.81MB/s
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
    Reqs/sec     73164.22    5251.09   83784.04
    Latency      682.19us   159.68us     8.21ms
    HTTP codes:
      1xx - 0, 2xx - 97747, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2253
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2251
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:    11.30MB/s
  ```


