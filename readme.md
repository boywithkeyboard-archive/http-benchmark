## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `130124` | `4987` | `145775` |
| **91%** | [Hyper Express](#hyper-express) | `117860` | `4667` | `120812` |
| **55%** | [Fastify](#fastify) | `72053` | `18739` | `89843` |
| **54%** | [Node (Default)](#node-default) | `70392` | `17713` | `110361` |
| **52%** | [Hono](#hono) | `68182` | `19841` | `85447` |
| **51%** | [Koa](#koa) | `66063` | `19966` | `122723` |
| **20%** | [Carbon](#carbon) | `26332` | `7187` | `39100` |
| **15%** | [Express](#express) | `19347` | `4376` | `26172` |


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
    Reqs/sec     27079.50   14615.99  136500.55
    Latency        1.84ms     2.77ms   232.16ms
    HTTP codes:
      1xx - 0, 2xx - 91543, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8457
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8457
    Throughput:     5.65MB/s
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
    Reqs/sec     19959.58   12174.61  126565.96
    Latency        2.50ms     2.15ms   192.46ms
    HTTP codes:
      1xx - 0, 2xx - 91012, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8988
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8988
    Throughput:     5.20MB/s
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
    Reqs/sec     74144.84   19942.80  120927.11
    Latency      672.60us   784.24us    64.43ms
    HTTP codes:
      1xx - 0, 2xx - 86461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13539
    Throughput:    14.54MB/s
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
    Reqs/sec     72053.94   21156.04  121246.32
    Latency      692.62us     1.23ms    87.97ms
    HTTP codes:
      1xx - 0, 2xx - 91675, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8325
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8325
    Throughput:    14.91MB/s
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
    Reqs/sec    122034.86    5340.63  139624.29
    Latency      407.48us   191.68us     7.97ms
    HTTP codes:
      1xx - 0, 2xx - 90142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9858
    Throughput:    15.63MB/s
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
    Reqs/sec     63229.81   18980.49  120346.12
    Latency      785.76us     1.04ms    81.09ms
    HTTP codes:
      1xx - 0, 2xx - 92390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7610
    Throughput:    13.24MB/s
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
    Reqs/sec     73775.04   16252.28  140940.21
    Latency      674.91us   783.17us    60.13ms
    HTTP codes:
      1xx - 0, 2xx - 95370, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4630
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4630
    Throughput:    16.12MB/s
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
    Reqs/sec    120172.59   12526.92  140357.50
    Latency      414.56us   115.26us     5.82ms
    HTTP codes:
      1xx - 0, 2xx - 94343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5657
    Throughput:    17.93MB/s
  ```


