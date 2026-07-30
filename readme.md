## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `180239` | `9537` | `188603` |
| **81%** | [Hyper Express](#hyper-express) | `146518` | `11383` | `163872` |
| **38%** | [Node (Default)](#node-default) | `67904` | `15262` | `142891` |
| **34%** | [Fastify](#fastify) | `60677` | `12474` | `76384` |
| **32%** | [Koa](#koa) | `57335` | `20963` | `157622` |
| **29%** | [Hono](#hono) | `52375` | `11843` | `75273` |
| **13%** | [Carbon](#carbon) | `22571` | `5007` | `30245` |
| **9%** | [Express](#express) | `15735` | `3066` | `21307` |


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
    Reqs/sec     25066.21   18225.66  161685.47
    Latency        1.99ms     2.74ms   224.96ms
    HTTP codes:
      1xx - 0, 2xx - 87929, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12071
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12071
    Throughput:     5.01MB/s
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
    Reqs/sec     18326.87   15038.06  158382.43
    Latency        2.72ms     1.98ms   185.25ms
    HTTP codes:
      1xx - 0, 2xx - 88788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11212
    Throughput:     4.66MB/s
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
    Reqs/sec     68800.60   30226.09  166935.34
    Latency      721.89us     0.90ms    70.18ms
    HTTP codes:
      1xx - 0, 2xx - 78643, 3xx - 0, 4xx - 0, 5xx - 0
      others - 21357
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 21357
    Throughput:    12.31MB/s
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
    Reqs/sec     59781.88   24768.43  160204.07
    Latency      832.91us     1.19ms    95.99ms
    HTTP codes:
      1xx - 0, 2xx - 87710, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12290
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12290
    Throughput:    11.87MB/s
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
    Reqs/sec    151710.45   10406.52  169128.34
    Latency      327.48us   154.52us     5.42ms
    HTTP codes:
      1xx - 0, 2xx - 89310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10690
    Throughput:    19.25MB/s
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
    Reqs/sec     57629.43   22112.32  152866.87
    Latency        0.86ms     1.00ms    75.95ms
    HTTP codes:
      1xx - 0, 2xx - 90187, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9813
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9813
    Throughput:    11.78MB/s
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
    Reqs/sec     66198.84   15365.50  158462.18
    Latency      752.77us   795.89us    57.30ms
    HTTP codes:
      1xx - 0, 2xx - 95269, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4731
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4731
    Throughput:    14.44MB/s
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
    Reqs/sec    177955.84   12851.92  188301.39
    Latency      279.40us   157.63us    10.08ms
    HTTP codes:
      1xx - 0, 2xx - 95059, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4941
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4941
    Throughput:    26.76MB/s
  ```


