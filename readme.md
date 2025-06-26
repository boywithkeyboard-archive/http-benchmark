## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73325` | `4198` | `80533` |
| **84%** | [Hyper Express](#hyper-express) | `61438` | `3005` | `63831` |
| **35%** | [Node (Default)](#node-default) | `25518` | `7397` | `53332` |
| **33%** | [Fastify](#fastify) | `24330` | `7722` | `37336` |
| **27%** | [Hono](#hono) | `20046` | `5533` | `29346` |
| **25%** | [Koa](#koa) | `18235` | `6989` | `57933` |
| **11%** | [Carbon](#carbon) | `8111` | `1416` | `10419` |
| **9%** | [Express](#express) | `6251` | `1017` | `8419` |


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
    Reqs/sec      8693.40    4342.72   52576.93
    Latency        5.74ms     4.37ms   372.75ms
    HTTP codes:
      1xx - 0, 2xx - 92908, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7092
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7092
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
    Reqs/sec      6330.16    1051.44    8337.61
    Latency        7.90ms     3.70ms   354.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     24155.62    7418.79   36727.07
    Latency        2.07ms     2.01ms   180.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     21409.37    6488.36   30022.71
    Latency        2.33ms     2.09ms   184.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     61071.78    3610.91   64063.43
    Latency      816.61us    76.64us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     19008.15    6907.56   52713.44
    Latency        2.63ms     2.44ms   215.42ms
    HTTP codes:
      1xx - 0, 2xx - 94033, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5967
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5967
    Throughput:     4.04MB/s
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
    Reqs/sec     24258.99    5829.96   50301.52
    Latency        2.06ms     1.99ms   168.87ms
    HTTP codes:
      1xx - 0, 2xx - 97255, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2745
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2745
    Throughput:     5.39MB/s
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
    Reqs/sec     73349.55    5171.24   81882.22
    Latency      678.62us   187.21us     6.66ms
    HTTP codes:
      1xx - 0, 2xx - 96825, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3175
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3175
    Throughput:    11.24MB/s
  ```


