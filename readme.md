## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73710` | `5716` | `92245` |
| **85%** | [Hyper Express](#hyper-express) | `62346` | `4001` | `69116` |
| **33%** | [Node (Default)](#node-default) | `24333` | `6391` | `46864` |
| **29%** | [Fastify](#fastify) | `21298` | `5043` | `35110` |
| **25%** | [Hono](#hono) | `18702` | `3793` | `29226` |
| **23%** | [Koa](#koa) | `17211` | `5194` | `50657` |
| **11%** | [Carbon](#carbon) | `8360` | `1442` | `10633` |
| **9%** | [Express](#express) | `6463` | `1026` | `8657` |


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
    Reqs/sec      8622.78    3687.34   51660.88
    Latency        5.79ms     4.21ms   365.53ms
    HTTP codes:
      1xx - 0, 2xx - 95043, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4957
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4957
    Throughput:     1.86MB/s
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
    Reqs/sec      6534.57    1054.29    8528.89
    Latency        7.65ms     3.49ms   334.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     21030.76    4700.67   34448.81
    Latency        2.38ms     2.01ms   182.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     18867.45    4080.39   29808.95
    Latency        2.65ms     1.90ms   171.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.26MB/s
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
    Reqs/sec     63029.36    3799.60   70164.48
    Latency      791.46us    78.35us     3.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     16277.92    4980.65   47693.74
    Latency        3.07ms     2.32ms   200.94ms
    HTTP codes:
      1xx - 0, 2xx - 94969, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5031
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5031
    Throughput:     3.50MB/s
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
    Reqs/sec     23134.78    5660.52   54474.77
    Latency        2.16ms     1.76ms   152.86ms
    HTTP codes:
      1xx - 0, 2xx - 97438, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2562
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2562
    Throughput:     5.15MB/s
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
    Reqs/sec     74069.38    5633.07   87336.56
    Latency      671.92us   244.91us    11.62ms
    HTTP codes:
      1xx - 0, 2xx - 96278, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3722
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3722
    Throughput:    11.28MB/s
  ```


