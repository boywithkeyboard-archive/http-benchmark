## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74285` | `5483` | `83503` |
| **84%** | [Hyper Express](#hyper-express) | `62130` | `3145` | `66494` |
| **35%** | [Node (Default)](#node-default) | `26302` | `7997` | `46398` |
| **32%** | [Fastify](#fastify) | `23568` | `6762` | `36801` |
| **27%** | [Hono](#hono) | `20368` | `5465` | `29622` |
| **26%** | [Koa](#koa) | `18997` | `6667` | `59587` |
| **11%** | [Carbon](#carbon) | `8361` | `1456` | `10373` |
| **9%** | [Express](#express) | `6601` | `1108` | `8387` |


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
    Reqs/sec      8832.09    5087.90   56045.35
    Latency        5.65ms     4.39ms   378.03ms
    HTTP codes:
      1xx - 0, 2xx - 91794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8206
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
    Reqs/sec      6850.35    4465.42   53935.59
    Latency        7.29ms     3.87ms   354.00ms
    HTTP codes:
      1xx - 0, 2xx - 91935, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8065
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8065
    Throughput:     1.80MB/s
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
    Reqs/sec     25068.91    8100.92   36504.56
    Latency        1.99ms     2.01ms   177.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
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
    Reqs/sec     20438.37    5915.23   29538.75
    Latency        2.45ms     2.07ms   185.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     62739.12    5403.32   84582.90
    Latency      794.36us    76.26us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     17832.59    6243.62   52094.29
    Latency        2.80ms     2.39ms   209.50ms
    HTTP codes:
      1xx - 0, 2xx - 94440, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5560
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5560
    Throughput:     3.81MB/s
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
    Reqs/sec     26125.27    7617.49   46604.19
    Latency        1.91ms     1.89ms   162.24ms
    HTTP codes:
      1xx - 0, 2xx - 97793, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2207
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2207
    Throughput:     5.84MB/s
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
    Reqs/sec     73522.38    5683.58   86315.62
    Latency      677.83us   149.78us     6.44ms
    HTTP codes:
      1xx - 0, 2xx - 97907, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2093
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2093
    Throughput:    11.39MB/s
  ```


