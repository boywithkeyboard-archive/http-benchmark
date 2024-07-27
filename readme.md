## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73904` | `5946` | `82167` |
| **83%** | [Hyper Express](#hyper-express) | `61236` | `3244` | `64226` |
| **33%** | [Node (Default)](#node-default) | `24621` | `7016` | `46045` |
| **32%** | [Fastify](#fastify) | `23950` | `7091` | `34978` |
| **27%** | [Hono](#hono) | `20109` | `5104` | `28896` |
| **24%** | [Koa](#koa) | `18057` | `6119` | `50497` |
| **11%** | [Carbon](#carbon) | `8028` | `1350` | `10329` |
| **9%** | [Express](#express) | `6428` | `1000` | `8385` |


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
    Reqs/sec      8583.07    4647.73   58016.37
    Latency        5.81ms     4.16ms   362.49ms
    HTTP codes:
      1xx - 0, 2xx - 93152, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6848
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6848
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
    Reqs/sec      6447.21    1023.92    8436.85
    Latency        7.75ms     3.58ms   343.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     22885.73    6718.84   35087.87
    Latency        2.18ms     2.05ms   184.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     20360.03    5505.21   28661.26
    Latency        2.45ms     2.03ms   181.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     62594.03    4503.73   73656.77
    Latency      796.59us    94.11us     4.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18241.51    6653.18   56788.94
    Latency        2.74ms     2.34ms   205.47ms
    HTTP codes:
      1xx - 0, 2xx - 94008, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5992
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5992
    Throughput:     3.88MB/s
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
    Reqs/sec     24885.09    6941.12   42713.66
    Latency        2.01ms     1.93ms   166.84ms
    HTTP codes:
      1xx - 0, 2xx - 98105, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1895
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1895
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
    Reqs/sec     74555.21    6152.83   87669.32
    Latency      667.44us   270.25us    14.55ms
    HTTP codes:
      1xx - 0, 2xx - 96845, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3155
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3155
    Throughput:    11.42MB/s
  ```


