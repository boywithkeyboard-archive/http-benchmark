## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78192` | `7302` | `86256` |
| **80%** | [Hyper Express](#hyper-express) | `62330` | `3317` | `72553` |
| **33%** | [Node (Default)](#node-default) | `26182` | `8497` | `60504` |
| **31%** | [Fastify](#fastify) | `24107` | `7154` | `35777` |
| **26%** | [Hono](#hono) | `20020` | `5121` | `29006` |
| **25%** | [Koa](#koa) | `19409` | `7311` | `57403` |
| **11%** | [Carbon](#carbon) | `8226` | `1401` | `10463` |
| **8%** | [Express](#express) | `6387` | `1011` | `8551` |


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
    Reqs/sec      8917.89    4637.09   52946.38
    Latency        5.58ms     4.32ms   382.00ms
    HTTP codes:
      1xx - 0, 2xx - 92532, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7468
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7468
    Throughput:     1.88MB/s
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
    Reqs/sec      7113.73    5219.31   53084.16
    Latency        7.02ms     3.84ms   349.33ms
    HTTP codes:
      1xx - 0, 2xx - 89253, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10747
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10734
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 13
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
    Reqs/sec     24046.48    7337.34   35365.77
    Latency        2.08ms     2.00ms   180.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20016.09    5609.17   29824.01
    Latency        2.50ms     2.00ms   181.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     62262.26    3928.65   67846.96
    Latency      800.91us    75.91us     4.10ms
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
    Reqs/sec     19468.48    7521.19   53071.33
    Latency        2.56ms     2.40ms   209.53ms
    HTTP codes:
      1xx - 0, 2xx - 90503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9497
    Throughput:     3.98MB/s
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
    Reqs/sec     26086.46    7701.45   54666.08
    Latency        1.91ms     1.94ms   167.11ms
    HTTP codes:
      1xx - 0, 2xx - 97260, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2740
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2740
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
    Reqs/sec     75054.74    5742.80   82224.63
    Latency      664.12us   159.09us    10.09ms
    HTTP codes:
      1xx - 0, 2xx - 97268, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2732
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2732
    Throughput:    11.55MB/s
  ```


