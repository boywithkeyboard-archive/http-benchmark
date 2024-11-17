## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75254` | `7608` | `92444` |
| **84%** | [Hyper Express](#hyper-express) | `63042` | `3858` | `73825` |
| **35%** | [Node (Default)](#node-default) | `25967` | `7683` | `45264` |
| **30%** | [Fastify](#fastify) | `22795` | `6112` | `35101` |
| **29%** | [Hono](#hono) | `21459` | `6079` | `29548` |
| **23%** | [Koa](#koa) | `17631` | `5955` | `57889` |
| **11%** | [Carbon](#carbon) | `8235` | `1387` | `10409` |
| **9%** | [Express](#express) | `6555` | `1052` | `8497` |


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
    Reqs/sec      8506.87    3599.25   47244.81
    Latency        5.87ms     4.22ms   368.44ms
    HTTP codes:
      1xx - 0, 2xx - 95103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4897
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
    Reqs/sec      6430.11     974.77    8525.47
    Latency        7.77ms     3.59ms   343.17ms
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
    Reqs/sec     23279.03    6643.90   34079.97
    Latency        2.15ms     1.91ms   172.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.28MB/s
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
    Reqs/sec     20280.74    5349.97   28356.81
    Latency        2.46ms     1.93ms   176.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     62717.06    4127.74   71820.63
    Latency      795.20us    71.52us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     18205.06    5936.07   48610.34
    Latency        2.74ms     2.33ms   201.61ms
    HTTP codes:
      1xx - 0, 2xx - 95390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4610
    Throughput:     3.93MB/s
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
    Reqs/sec     24689.67    6534.30   39462.60
    Latency        2.02ms     1.96ms   167.06ms
    HTTP codes:
      1xx - 0, 2xx - 98386, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1614
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1614
    Throughput:     5.57MB/s
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
    Reqs/sec     73942.93    5869.14   83782.45
    Latency      673.64us   240.04us    18.42ms
    HTTP codes:
      1xx - 0, 2xx - 96157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3843
    Throughput:    11.25MB/s
  ```


