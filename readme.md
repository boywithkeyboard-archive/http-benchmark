## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72703` | `4629` | `83391` |
| **83%** | [Hyper Express](#hyper-express) | `60281` | `3201` | `63013` |
| **34%** | [Node (Default)](#node-default) | `24533` | `7175` | `54726` |
| **33%** | [Fastify](#fastify) | `23661` | `7033` | `35763` |
| **29%** | [Hono](#hono) | `20972` | `6157` | `30027` |
| **26%** | [Koa](#koa) | `19158` | `7554` | `63514` |
| **11%** | [Carbon](#carbon) | `8233` | `1411` | `10435` |
| **9%** | [Express](#express) | `6355` | `1007` | `8448` |


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
    Reqs/sec      8997.26    5292.58   57943.99
    Latency        5.54ms     4.34ms   372.90ms
    HTTP codes:
      1xx - 0, 2xx - 91385, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8615
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8615
    Throughput:     1.87MB/s
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
    Reqs/sec      6585.85    1140.45    8348.34
    Latency        7.59ms     3.80ms   362.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24322.09    7284.98   35860.41
    Latency        2.06ms     2.09ms   184.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21046.90    6203.50   29994.57
    Latency        2.37ms     2.06ms   182.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     60383.11    3700.28   77975.16
    Latency      828.37us    68.96us     2.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.55MB/s
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
    Reqs/sec     18838.68    6886.19   55221.94
    Latency        2.65ms     2.54ms   221.60ms
    HTTP codes:
      1xx - 0, 2xx - 93228, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6772
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6772
    Throughput:     3.97MB/s
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
    Reqs/sec     24272.81    7133.82   48771.27
    Latency        2.06ms     1.94ms   167.22ms
    HTTP codes:
      1xx - 0, 2xx - 97453, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2547
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2547
    Throughput:     5.42MB/s
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
    Reqs/sec     72289.53    5145.08   85522.26
    Latency      689.37us   169.63us     8.08ms
    HTTP codes:
      1xx - 0, 2xx - 97813, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2187
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2187
    Throughput:    11.18MB/s
  ```


