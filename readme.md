## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71203` | `4660` | `82385` |
| **87%** | [Hyper Express](#hyper-express) | `61978` | `3108` | `64157` |
| **38%** | [Node (Default)](#node-default) | `27317` | `8071` | `56056` |
| **35%** | [Fastify](#fastify) | `24876` | `7665` | `36673` |
| **30%** | [Hono](#hono) | `21411` | `5900` | `30648` |
| **27%** | [Koa](#koa) | `19485` | `6562` | `52509` |
| **12%** | [Carbon](#carbon) | `8269` | `1420` | `10443` |
| **9%** | [Express](#express) | `6524` | `1054` | `8483` |


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
    Reqs/sec      8500.03    3757.44   50181.70
    Latency        5.87ms     4.24ms   369.90ms
    HTTP codes:
      1xx - 0, 2xx - 95105, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4895
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4895
    Throughput:     1.84MB/s
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
    Reqs/sec      6407.88    1014.74    8360.03
    Latency        7.80ms     3.70ms   351.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     25220.51    7829.58   37066.61
    Latency        1.98ms     2.02ms   181.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.72MB/s
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
    Reqs/sec     20499.51    5330.01   30072.21
    Latency        2.44ms     1.96ms   176.32ms
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
    Reqs/sec     61515.54    3867.81   64863.43
    Latency      808.87us    75.90us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     18765.00    6118.11   50888.59
    Latency        2.66ms     2.38ms   208.11ms
    HTTP codes:
      1xx - 0, 2xx - 94915, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5085
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5085
    Throughput:     4.03MB/s
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
    Reqs/sec     27523.99    8301.86   58324.77
    Latency        1.81ms     1.78ms   158.03ms
    HTTP codes:
      1xx - 0, 2xx - 96791, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3209
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3209
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
    Reqs/sec     72798.70    4421.16   81584.48
    Latency      684.24us   188.59us     8.56ms
    HTTP codes:
      1xx - 0, 2xx - 96266, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3734
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3734
    Throughput:    11.09MB/s
  ```


