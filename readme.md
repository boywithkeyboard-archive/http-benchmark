## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74990` | `6505` | `91500` |
| **85%** | [Hyper Express](#hyper-express) | `63412` | `3987` | `70493` |
| **35%** | [Node (Default)](#node-default) | `25944` | `7068` | `52277` |
| **34%** | [Fastify](#fastify) | `25133` | `7532` | `37577` |
| **28%** | [Hono](#hono) | `20884` | `5882` | `30456` |
| **26%** | [Koa](#koa) | `19459` | `7062` | `57337` |
| **11%** | [Carbon](#carbon) | `8368` | `1449` | `10568` |
| **9%** | [Express](#express) | `6666` | `1147` | `8547` |


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
    Reqs/sec      8834.37    4444.51   61322.32
    Latency        5.64ms     4.27ms   368.98ms
    HTTP codes:
      1xx - 0, 2xx - 93232, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6768
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6768
    Throughput:     1.87MB/s
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
    Reqs/sec      7143.14    4101.25   49432.92
    Latency        6.98ms     3.92ms   357.72ms
    HTTP codes:
      1xx - 0, 2xx - 92993, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7007
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7007
    Throughput:     1.90MB/s
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
    Reqs/sec     25156.67    7943.21   37119.73
    Latency        1.99ms     1.92ms   171.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     20510.43    5359.98   30426.99
    Latency        2.44ms     2.05ms   183.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     62246.00    3810.09   66620.47
    Latency      801.20us    68.63us     3.21ms
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
    Reqs/sec     18928.63    6841.74   55762.81
    Latency        2.64ms     2.39ms   208.64ms
    HTTP codes:
      1xx - 0, 2xx - 94063, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5937
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5937
    Throughput:     4.02MB/s
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
    Reqs/sec     27466.77    8158.31   56694.28
    Latency        1.82ms     1.87ms   160.15ms
    HTTP codes:
      1xx - 0, 2xx - 97249, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2751
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2751
    Throughput:     6.10MB/s
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
    Reqs/sec     75134.67    6016.03   82929.82
    Latency      663.40us   152.62us     6.89ms
    HTTP codes:
      1xx - 0, 2xx - 97759, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2241
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2241
    Throughput:    11.62MB/s
  ```


