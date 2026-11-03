## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68944` | `4682` | `84906` |
| **84%** | [Hyper Express](#hyper-express) | `57652` | `3248` | `62424` |
| **35%** | [Node (Default)](#node-default) | `24011` | `8468` | `56755` |
| **32%** | [Fastify](#fastify) | `21943` | `6399` | `31808` |
| **27%** | [Koa](#koa) | `18635` | `10490` | `75457` |
| **26%** | [Hono](#hono) | `18115` | `5343` | `27365` |
| **11%** | [Carbon](#carbon) | `7615` | `1253` | `10075` |
| **9%** | [Express](#express) | `6186` | `1060` | `8191` |


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
    Reqs/sec      8103.22    5171.92   73985.58
    Latency        6.16ms     4.66ms   394.20ms
    HTTP codes:
      1xx - 0, 2xx - 92433, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7567
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7567
    Throughput:     1.70MB/s
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
    Reqs/sec      6028.63     992.28    8178.56
    Latency        8.29ms     3.91ms   371.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.72MB/s
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
    Reqs/sec     23712.92    6778.80   31813.80
    Latency        2.11ms     2.08ms   185.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     18556.57    5492.27   27139.40
    Latency        2.69ms     2.21ms   199.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.19MB/s
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
    Reqs/sec     58326.03    4391.90   82837.40
    Latency        0.86ms    90.66us     3.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     17376.15    7852.13   73025.80
    Latency        2.87ms     2.59ms   224.47ms
    HTTP codes:
      1xx - 0, 2xx - 91913, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8087
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8087
    Throughput:     3.62MB/s
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
    Reqs/sec     24535.78    7689.73   57591.32
    Latency        2.03ms     2.02ms   174.62ms
    HTTP codes:
      1xx - 0, 2xx - 97539, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2461
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2461
    Throughput:     5.48MB/s
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
    Reqs/sec     70059.71    3642.46   85334.33
    Latency      712.02us   181.42us     6.95ms
    HTTP codes:
      1xx - 0, 2xx - 95203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4797
    Throughput:    10.54MB/s
  ```


