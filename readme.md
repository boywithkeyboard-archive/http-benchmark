## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73639` | `9190` | `85895` |
| **85%** | [Hyper Express](#hyper-express) | `62466` | `4387` | `76680` |
| **35%** | [Node (Default)](#node-default) | `25811` | `7549` | `42560` |
| **33%** | [Fastify](#fastify) | `24294` | `7092` | `35707` |
| **30%** | [Hono](#hono) | `21794` | `6317` | `30312` |
| **24%** | [Koa](#koa) | `17908` | `6186` | `48499` |
| **11%** | [Carbon](#carbon) | `8327` | `1456` | `10491` |
| **8%** | [Express](#express) | `6174` | `949` | `8360` |


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
    Reqs/sec      8410.64    3383.62   42999.06
    Latency        5.93ms     4.38ms   382.08ms
    HTTP codes:
      1xx - 0, 2xx - 95347, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4653
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4653
    Throughput:     1.82MB/s
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
    Reqs/sec      6377.48    1083.52    8383.31
    Latency        7.84ms     3.78ms   362.55ms
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
    Reqs/sec     24043.19    7719.22   35594.56
    Latency        2.08ms     2.03ms   185.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     21273.21    6106.91   29965.61
    Latency        2.35ms     2.02ms   183.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     61757.52    3354.57   68284.05
    Latency      807.33us    63.80us     2.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     19507.76    6769.23   54938.15
    Latency        2.56ms     2.38ms   208.46ms
    HTTP codes:
      1xx - 0, 2xx - 94334, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5666
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5666
    Throughput:     4.16MB/s
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
    Reqs/sec     25390.88    7011.75   39753.34
    Latency        1.97ms     2.00ms   174.94ms
    HTTP codes:
      1xx - 0, 2xx - 98468, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1532
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1532
    Throughput:     5.72MB/s
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
    Reqs/sec     73728.41    6435.36   83632.49
    Latency      675.17us   294.50us    13.93ms
    HTTP codes:
      1xx - 0, 2xx - 96500, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3500
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3500
    Throughput:    11.26MB/s
  ```


