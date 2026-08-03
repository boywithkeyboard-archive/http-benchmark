## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78252` | `2434` | `82141` |
| **87%** | [Hyper Express](#hyper-express) | `68125` | `3497` | `74051` |
| **44%** | [Fastify](#fastify) | `34730` | `11292` | `50859` |
| **44%** | [Node (Default)](#node-default) | `34513` | `9096` | `67226` |
| **38%** | [Hono](#hono) | `29372` | `9086` | `46193` |
| **37%** | [Koa](#koa) | `28967` | `11744` | `73275` |
| **12%** | [Carbon](#carbon) | `9208` | `2278` | `13570` |
| **9%** | [Express](#express) | `7244` | `1622` | `11772` |


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
    Reqs/sec     10252.90    7018.79   80492.21
    Latency        4.86ms     4.35ms   374.73ms
    HTTP codes:
      1xx - 0, 2xx - 91111, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8889
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8889
    Throughput:     2.12MB/s
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
    Reqs/sec      8306.71    6433.01   79311.35
    Latency        6.01ms     3.93ms   360.38ms
    HTTP codes:
      1xx - 0, 2xx - 90382, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9618
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9618
    Throughput:     2.15MB/s
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
    Reqs/sec     32628.96    9256.81   50966.95
    Latency        1.53ms     1.94ms   168.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.40MB/s
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
    Reqs/sec     31026.78   10320.14   44578.71
    Latency        1.61ms     2.08ms   182.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.01MB/s
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
    Reqs/sec     70410.27    4008.98   77466.63
    Latency      709.12us    69.92us     2.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.99MB/s
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
    Reqs/sec     29587.96   12856.43   83825.58
    Latency        1.69ms     2.33ms   200.07ms
    HTTP codes:
      1xx - 0, 2xx - 91673, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8327
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8327
    Throughput:     6.12MB/s
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
    Reqs/sec     35557.01    9780.16   70207.13
    Latency        1.40ms     1.83ms   155.11ms
    HTTP codes:
      1xx - 0, 2xx - 97121, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2879
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2879
    Throughput:     7.92MB/s
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
    Reqs/sec     78030.39    3454.26   84516.95
    Latency      638.83us   152.22us     8.45ms
    HTTP codes:
      1xx - 0, 2xx - 97489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2511
    Throughput:    12.04MB/s
  ```


