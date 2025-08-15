## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76763` | `2937` | `85179` |
| **83%** | [Hyper Express](#hyper-express) | `63782` | `3760` | `67188` |
| **34%** | [Node (Default)](#node-default) | `26364` | `8405` | `62764` |
| **33%** | [Fastify](#fastify) | `25542` | `8028` | `36418` |
| **28%** | [Hono](#hono) | `21250` | `6245` | `30768` |
| **24%** | [Koa](#koa) | `18391` | `8038` | `67266` |
| **11%** | [Carbon](#carbon) | `8119` | `1411` | `10425` |
| **9%** | [Express](#express) | `6574` | `1080` | `8458` |


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
    Reqs/sec      8950.46    5203.24   71909.43
    Latency        5.58ms     4.21ms   362.17ms
    HTTP codes:
      1xx - 0, 2xx - 93380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6620
    Throughput:     1.90MB/s
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
    Reqs/sec      7679.93    7943.40   82295.63
    Latency        6.50ms     3.94ms   359.21ms
    HTTP codes:
      1xx - 0, 2xx - 85659, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14341
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14341
    Throughput:     1.88MB/s
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
    Reqs/sec     24540.77    8091.81   37043.83
    Latency        2.04ms     2.05ms   183.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     21135.93    5692.71   28695.89
    Latency        2.36ms     2.00ms   178.76ms
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
    Reqs/sec     63503.95    3376.80   67461.90
    Latency      785.12us    71.21us     3.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     19630.72    9684.88   76654.27
    Latency        2.54ms     2.21ms   197.74ms
    HTTP codes:
      1xx - 0, 2xx - 89959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10041
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
    Reqs/sec     26199.12    8570.37   77689.61
    Latency        1.91ms     1.87ms   159.08ms
    HTTP codes:
      1xx - 0, 2xx - 96507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3493
    Throughput:     5.78MB/s
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
    Reqs/sec     74873.18    2278.92   83207.74
    Latency      664.87us   195.78us    10.93ms
    HTTP codes:
      1xx - 0, 2xx - 96462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3538
    Throughput:    11.43MB/s
  ```


