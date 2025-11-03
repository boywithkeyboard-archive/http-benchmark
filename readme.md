## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74366` | `5181` | `88999` |
| **83%** | [Hyper Express](#hyper-express) | `62082` | `3954` | `65406` |
| **35%** | [Node (Default)](#node-default) | `25993` | `8271` | `76422` |
| **31%** | [Fastify](#fastify) | `23232` | `6587` | `36718` |
| **28%** | [Hono](#hono) | `21039` | `6103` | `31173` |
| **26%** | [Koa](#koa) | `19638` | `7933` | `67788` |
| **11%** | [Carbon](#carbon) | `8364` | `1496` | `10570` |
| **9%** | [Express](#express) | `6481` | `1095` | `8506` |


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
    Reqs/sec      9141.61    7317.13   77652.55
    Latency        5.46ms     4.29ms   370.82ms
    HTTP codes:
      1xx - 0, 2xx - 88536, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11464
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11464
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
    Reqs/sec      7031.50    5651.88   70450.85
    Latency        7.10ms     3.95ms   360.21ms
    HTTP codes:
      1xx - 0, 2xx - 90687, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9313
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9313
    Throughput:     1.83MB/s
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
    Reqs/sec     24511.08    7642.17   35764.60
    Latency        2.04ms     2.05ms   183.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     20209.20    5502.02   29239.94
    Latency        2.47ms     2.05ms   183.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     62528.74    3873.09   68071.29
    Latency      797.65us    78.05us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18997.21    8922.19   76537.88
    Latency        2.63ms     2.42ms   210.53ms
    HTTP codes:
      1xx - 0, 2xx - 91302, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8698
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8698
    Throughput:     3.92MB/s
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
    Reqs/sec     25023.96    7629.66   67035.51
    Latency        1.99ms     1.95ms   169.47ms
    HTTP codes:
      1xx - 0, 2xx - 96675, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3325
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3325
    Throughput:     5.54MB/s
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
    Reqs/sec     75398.55    2611.50   79798.09
    Latency      660.58us   173.50us    13.06ms
    HTTP codes:
      1xx - 0, 2xx - 96705, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3295
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3295
    Throughput:    11.54MB/s
  ```


