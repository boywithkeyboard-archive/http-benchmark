## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70045` | `4404` | `80557` |
| **84%** | [Hyper Express](#hyper-express) | `58760` | `3797` | `67536` |
| **30%** | [Hono](#hono) | `20703` | `6448` | `30495` |
| **29%** | [Node (Default)](#node-default) | `20594` | `5473` | `60345` |
| **29%** | [Fastify](#fastify) | `20581` | `5566` | `35636` |
| **26%** | [Koa](#koa) | `18368` | `8031` | `65983` |
| **10%** | [Carbon](#carbon) | `7271` | `1219` | `10121` |
| **9%** | [Express](#express) | `6073` | `1100` | `8362` |


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
    Reqs/sec      7734.59    5063.30   65947.53
    Latency        6.45ms     4.65ms   402.75ms
    HTTP codes:
      1xx - 0, 2xx - 92744, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7256
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7256
    Throughput:     1.63MB/s
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
    Reqs/sec      6113.67    1111.04    8087.39
    Latency        8.17ms     3.99ms   382.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20109.75    5336.57   34557.03
    Latency        2.49ms     2.06ms   187.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     21810.45    6928.95   29874.78
    Latency        2.29ms     2.06ms   186.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     58267.87    2782.51   64374.17
    Latency        0.86ms    84.83us     3.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.28MB/s
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
    Reqs/sec     16758.06    8730.75   79454.08
    Latency        2.98ms     2.63ms   226.04ms
    HTTP codes:
      1xx - 0, 2xx - 91136, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8864
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8864
    Throughput:     3.45MB/s
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
    Reqs/sec     21851.13    8589.37   82384.10
    Latency        2.28ms     2.00ms   169.54ms
    HTTP codes:
      1xx - 0, 2xx - 94197, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5803
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5803
    Throughput:     4.72MB/s
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
    Reqs/sec     70554.99    4748.91   83455.61
    Latency      704.53us   246.67us    15.30ms
    HTTP codes:
      1xx - 0, 2xx - 95946, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4054
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4054
    Throughput:    10.72MB/s
  ```


