## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73685` | `5115` | `90680` |
| **82%** | [Hyper Express](#hyper-express) | `60437` | `4082` | `75195` |
| **29%** | [Node (Default)](#node-default) | `21555` | `6308` | `64939` |
| **28%** | [Fastify](#fastify) | `20976` | `6149` | `36433` |
| **28%** | [Hono](#hono) | `20630` | `5954` | `29437` |
| **24%** | [Koa](#koa) | `17330` | `8936` | `77721` |
| **10%** | [Carbon](#carbon) | `7311` | `1228` | `10312` |
| **8%** | [Express](#express) | `6159` | `1084` | `8160` |


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
    Reqs/sec      7947.05    4821.94   62714.68
    Latency        6.28ms     4.53ms   384.84ms
    HTTP codes:
      1xx - 0, 2xx - 93454, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6546
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6546
    Throughput:     1.69MB/s
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
    Reqs/sec      6141.25    1085.56    8145.97
    Latency        8.14ms     3.87ms   373.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     21737.07    6605.86   36086.03
    Latency        2.30ms     2.02ms   184.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     21461.72    6252.68   29711.69
    Latency        2.33ms     1.97ms   180.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     60349.29    3523.55   65418.66
    Latency      826.27us    93.04us     3.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.57MB/s
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
    Reqs/sec     18363.89    8516.61   68937.76
    Latency        2.71ms     2.43ms   213.24ms
    HTTP codes:
      1xx - 0, 2xx - 91995, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8005
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8005
    Throughput:     3.82MB/s
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
    Reqs/sec     20987.04    5761.79   59479.30
    Latency        2.38ms     1.91ms   165.60ms
    HTTP codes:
      1xx - 0, 2xx - 97168, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2832
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2832
    Throughput:     4.67MB/s
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
    Reqs/sec     72554.91    5039.75   89856.75
    Latency      686.82us   239.79us    10.86ms
    HTTP codes:
      1xx - 0, 2xx - 96979, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3021
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3021
    Throughput:    11.13MB/s
  ```


