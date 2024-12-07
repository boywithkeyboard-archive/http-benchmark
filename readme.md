## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72493` | `5431` | `82693` |
| **86%** | [Hyper Express](#hyper-express) | `62510` | `4323` | `74594` |
| **35%** | [Node (Default)](#node-default) | `25119` | `6864` | `38492` |
| **31%** | [Fastify](#fastify) | `22713` | `6562` | `35755` |
| **28%** | [Hono](#hono) | `20238` | `5679` | `29888` |
| **25%** | [Koa](#koa) | `17992` | `6098` | `56250` |
| **11%** | [Carbon](#carbon) | `8269` | `1469` | `10595` |
| **9%** | [Express](#express) | `6523` | `1015` | `8532` |


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
    Reqs/sec      8501.35    3395.66   46014.20
    Latency        5.87ms     4.18ms   363.10ms
    HTTP codes:
      1xx - 0, 2xx - 95663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4337
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
    Reqs/sec      6513.72    1050.30    8527.80
    Latency        7.67ms     3.46ms   335.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23811.74    7003.36   35554.36
    Latency        2.10ms     1.95ms   176.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     20554.28    5524.09   29659.49
    Latency        2.43ms     1.91ms   171.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     61808.20    3198.58   72137.20
    Latency      806.68us    69.27us     3.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     17973.76    6513.70   61870.73
    Latency        2.78ms     2.27ms   200.78ms
    HTTP codes:
      1xx - 0, 2xx - 94138, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5862
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5862
    Throughput:     3.83MB/s
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
    Reqs/sec     25061.37    6721.01   64846.84
    Latency        1.99ms     1.88ms   160.43ms
    HTTP codes:
      1xx - 0, 2xx - 97103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2897
    Throughput:     5.58MB/s
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
    Reqs/sec     70795.45    5243.29   79345.63
    Latency      703.91us   192.51us    12.07ms
    HTTP codes:
      1xx - 0, 2xx - 97935, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2065
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2065
    Throughput:    10.97MB/s
  ```


