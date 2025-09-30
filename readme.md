## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73180` | `2350` | `76285` |
| **85%** | [Hyper Express](#hyper-express) | `62128` | `3348` | `65504` |
| **35%** | [Node (Default)](#node-default) | `25713` | `7943` | `72714` |
| **32%** | [Fastify](#fastify) | `23554` | `6987` | `36184` |
| **28%** | [Hono](#hono) | `20329` | `5450` | `29257` |
| **25%** | [Koa](#koa) | `18486` | `8459` | `78681` |
| **11%** | [Carbon](#carbon) | `8170` | `1465` | `10636` |
| **9%** | [Express](#express) | `6269` | `983` | `8240` |


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
    Reqs/sec      9013.08    6431.59   75073.76
    Latency        5.54ms     4.32ms   371.37ms
    HTTP codes:
      1xx - 0, 2xx - 91044, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8956
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8956
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
    Reqs/sec      6227.40     971.16    8238.11
    Latency        8.03ms     3.55ms   343.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     23758.70    7358.20   35738.43
    Latency        2.10ms     2.03ms   183.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     20428.86    5675.63   29447.70
    Latency        2.44ms     2.05ms   181.87ms
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
    Reqs/sec     61826.22    3589.97   64928.50
    Latency      806.60us    75.24us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     19421.34    9333.23   80829.55
    Latency        2.57ms     2.38ms   210.07ms
    HTTP codes:
      1xx - 0, 2xx - 90934, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9066
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9066
    Throughput:     3.99MB/s
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
    Reqs/sec     25732.46    7513.89   57997.46
    Latency        1.94ms     1.90ms   164.71ms
    HTTP codes:
      1xx - 0, 2xx - 97659, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2341
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2341
    Throughput:     5.76MB/s
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
    Reqs/sec     73830.79    2091.49   80372.09
    Latency      674.02us   179.64us     9.12ms
    HTTP codes:
      1xx - 0, 2xx - 95890, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4110
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4110
    Throughput:    11.21MB/s
  ```


