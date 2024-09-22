## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72452` | `4405` | `82021` |
| **86%** | [Hyper Express](#hyper-express) | `62103` | `4209` | `74062` |
| **38%** | [Node (Default)](#node-default) | `27250` | `8119` | `55644` |
| **34%** | [Fastify](#fastify) | `24849` | `7608` | `37032` |
| **28%** | [Hono](#hono) | `20286` | `5674` | `28809` |
| **27%** | [Koa](#koa) | `19372` | `7353` | `63804` |
| **12%** | [Carbon](#carbon) | `8349` | `1459` | `10393` |
| **9%** | [Express](#express) | `6229` | `989` | `8295` |


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
    Reqs/sec      8616.56    4561.32   58499.23
    Latency        5.79ms     4.41ms   383.22ms
    HTTP codes:
      1xx - 0, 2xx - 93380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6620
    Throughput:     1.83MB/s
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
    Reqs/sec      6829.25    4618.35   57882.66
    Latency        7.31ms     4.08ms   371.07ms
    HTTP codes:
      1xx - 0, 2xx - 91435, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8565
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8565
    Throughput:     1.79MB/s
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
    Reqs/sec     25455.09    7811.33   37144.78
    Latency        1.96ms     2.14ms   190.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.77MB/s
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
    Reqs/sec     21110.34    5940.68   29479.12
    Latency        2.37ms     2.01ms   180.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61345.67    3712.48   67719.65
    Latency      812.99us    89.53us     3.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     18859.38    7455.56   56453.27
    Latency        2.65ms     2.43ms   212.71ms
    HTTP codes:
      1xx - 0, 2xx - 92513, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7487
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7487
    Throughput:     3.95MB/s
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
    Reqs/sec     27158.45    8063.52   45207.56
    Latency        1.84ms     1.96ms   167.18ms
    HTTP codes:
      1xx - 0, 2xx - 97976, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2024
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2024
    Throughput:     6.08MB/s
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
    Reqs/sec     73286.26    5461.69   85614.30
    Latency      679.52us   167.63us     9.37ms
    HTTP codes:
      1xx - 0, 2xx - 97315, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2685
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2685
    Throughput:    11.29MB/s
  ```


