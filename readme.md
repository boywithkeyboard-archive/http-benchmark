## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72341` | `5485` | `95562` |
| **82%** | [Hyper Express](#hyper-express) | `59521` | `3677` | `67104` |
| **30%** | [Node (Default)](#node-default) | `21458` | `6167` | `78029` |
| **29%** | [Hono](#hono) | `21195` | `6850` | `31153` |
| **29%** | [Fastify](#fastify) | `21033` | `6260` | `36540` |
| **26%** | [Koa](#koa) | `18854` | `8716` | `70680` |
| **10%** | [Carbon](#carbon) | `7385` | `1236` | `10363` |
| **9%** | [Express](#express) | `6254` | `1128` | `8326` |


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
    Reqs/sec      7606.47    4365.41   63507.58
    Latency        6.57ms     4.34ms   373.79ms
    HTTP codes:
      1xx - 0, 2xx - 94027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5973
    Throughput:     1.62MB/s
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
    Reqs/sec      6196.94    1147.75    8350.15
    Latency        8.07ms     3.96ms   374.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     20801.87    5616.84   37132.10
    Latency        2.40ms     2.11ms   190.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     20950.35    6413.96   30266.65
    Latency        2.39ms     2.13ms   185.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     59287.96    3609.88   68646.97
    Latency      841.59us    91.38us     3.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.42MB/s
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
    Reqs/sec     19219.64    8682.33   65030.63
    Latency        2.59ms     2.48ms   217.86ms
    HTTP codes:
      1xx - 0, 2xx - 93120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6880
    Throughput:     4.05MB/s
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
    Reqs/sec     20408.42    5352.32   64993.41
    Latency        2.44ms     1.88ms   160.93ms
    HTTP codes:
      1xx - 0, 2xx - 96622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3378
    Throughput:     4.52MB/s
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
    Reqs/sec     70497.21    4366.77   83691.17
    Latency      705.63us   200.26us    10.90ms
    HTTP codes:
      1xx - 0, 2xx - 96581, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3419
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3419
    Throughput:    10.78MB/s
  ```


