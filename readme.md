## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69171` | `5013` | `86854` |
| **84%** | [Hyper Express](#hyper-express) | `58009` | `2596` | `61591` |
| **30%** | [Node (Default)](#node-default) | `20637` | `5211` | `62254` |
| **30%** | [Fastify](#fastify) | `20479` | `5351` | `35376` |
| **30%** | [Hono](#hono) | `20418` | `5945` | `29169` |
| **27%** | [Koa](#koa) | `18722` | `7866` | `62994` |
| **11%** | [Carbon](#carbon) | `7393` | `1304` | `12147` |
| **9%** | [Express](#express) | `6068` | `1096` | `8094` |


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
    Reqs/sec      7912.33    5193.61   64990.82
    Latency        6.31ms     4.87ms   418.02ms
    HTTP codes:
      1xx - 0, 2xx - 91869, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8131
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8131
    Throughput:     1.65MB/s
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
    Reqs/sec      6083.79    1093.55    8138.87
    Latency        8.21ms     3.84ms   366.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     20939.34    5866.84   36952.73
    Latency        2.39ms     2.10ms   186.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     21134.74    6279.24   28606.48
    Latency        2.36ms     2.37ms   205.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     57963.40    3866.36   67798.43
    Latency        0.86ms   114.42us     3.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.23MB/s
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
    Reqs/sec     19221.24    8166.23   67989.90
    Latency        2.60ms     2.50ms   214.62ms
    HTTP codes:
      1xx - 0, 2xx - 92865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7135
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
    Reqs/sec     20987.11    6949.72   72553.02
    Latency        2.37ms     2.03ms   174.47ms
    HTTP codes:
      1xx - 0, 2xx - 94482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5518
    Throughput:     4.55MB/s
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
    Reqs/sec     69603.71    4436.21   86331.76
    Latency      716.66us   175.28us    10.20ms
    HTTP codes:
      1xx - 0, 2xx - 96644, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3356
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3356
    Throughput:    10.61MB/s
  ```


