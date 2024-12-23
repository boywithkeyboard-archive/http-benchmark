## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76876` | `7382` | `96643` |
| **84%** | [Hyper Express](#hyper-express) | `64913` | `3901` | `68616` |
| **29%** | [Node (Default)](#node-default) | `22570` | `5458` | `56645` |
| **28%** | [Fastify](#fastify) | `21752` | `4941` | `35764` |
| **25%** | [Hono](#hono) | `19031` | `4186` | `30144` |
| **22%** | [Koa](#koa) | `16598` | `6022` | `62258` |
| **11%** | [Carbon](#carbon) | `8344` | `1432` | `10568` |
| **8%** | [Express](#express) | `6366` | `961` | `8539` |


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
    Reqs/sec      9166.90    5432.28   55512.46
    Latency        5.44ms     4.18ms   361.64ms
    HTTP codes:
      1xx - 0, 2xx - 91010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8990
    Throughput:     1.90MB/s
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
    Reqs/sec      6502.42    1046.69    8486.50
    Latency        7.68ms     3.69ms   352.10ms
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
    Reqs/sec     22266.55    5715.28   36283.01
    Latency        2.24ms     1.92ms   175.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.05MB/s
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
    Reqs/sec     19458.63    5003.17   30129.48
    Latency        2.57ms     1.88ms   171.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.40MB/s
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
    Reqs/sec     64839.50    4037.69   67834.45
    Latency      768.57us    73.73us     2.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.21MB/s
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
    Reqs/sec     16568.54    5253.77   52850.08
    Latency        3.01ms     2.39ms   208.37ms
    HTTP codes:
      1xx - 0, 2xx - 94485, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5515
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5515
    Throughput:     3.54MB/s
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
    Reqs/sec     23739.01    5700.66   41601.73
    Latency        2.11ms     1.89ms   163.94ms
    HTTP codes:
      1xx - 0, 2xx - 98343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1657
    Throughput:     5.33MB/s
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
    Reqs/sec     74330.42    5757.16   85819.09
    Latency      670.32us   191.62us    11.07ms
    HTTP codes:
      1xx - 0, 2xx - 98041, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1959
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1959
    Throughput:    11.52MB/s
  ```


