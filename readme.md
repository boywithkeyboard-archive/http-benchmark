## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69875` | `6573` | `114955` |
| **83%** | [Hyper Express](#hyper-express) | `57988` | `3225` | `63687` |
| **30%** | [Fastify](#fastify) | `20844` | `4981` | `36117` |
| **28%** | [Hono](#hono) | `19863` | `5877` | `30692` |
| **28%** | [Node (Default)](#node-default) | `19841` | `5967` | `73798` |
| **27%** | [Koa](#koa) | `18846` | `8062` | `62820` |
| **11%** | [Carbon](#carbon) | `7524` | `1361` | `14638` |
| **9%** | [Express](#express) | `6091` | `1019` | `8284` |


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
    Reqs/sec      8178.87    6279.78   76929.91
    Latency        6.10ms     4.37ms   376.41ms
    HTTP codes:
      1xx - 0, 2xx - 89786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10214
    Throughput:     1.67MB/s
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
    Reqs/sec      6131.38    1058.36    8744.25
    Latency        8.15ms     3.76ms   365.75ms
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
    Reqs/sec     20455.15    5483.58   36125.03
    Latency        2.44ms     2.11ms   191.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     21761.55    6935.51   30421.14
    Latency        2.29ms     2.15ms   192.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.92MB/s
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
    Reqs/sec     58165.51    3486.80   69874.68
    Latency        0.86ms    94.21us     2.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.25MB/s
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
    Reqs/sec     19075.50    8652.68   70601.22
    Latency        2.62ms     2.56ms   221.41ms
    HTTP codes:
      1xx - 0, 2xx - 91902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8098
    Throughput:     3.96MB/s
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
    Reqs/sec     19838.05    4699.20   59554.73
    Latency        2.52ms     1.86ms   159.92ms
    HTTP codes:
      1xx - 0, 2xx - 97255, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2745
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2745
    Throughput:     4.42MB/s
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
    Reqs/sec     69980.25    3798.86   83102.29
    Latency      711.05us   194.54us    10.64ms
    HTTP codes:
      1xx - 0, 2xx - 94345, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5655
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5655
    Throughput:    10.46MB/s
  ```


