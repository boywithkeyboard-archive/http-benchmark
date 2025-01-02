## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73700` | `5807` | `85447` |
| **86%** | [Hyper Express](#hyper-express) | `63216` | `3570` | `68467` |
| **34%** | [Node (Default)](#node-default) | `25149` | `7367` | `54779` |
| **33%** | [Fastify](#fastify) | `24029` | `6638` | `36014` |
| **28%** | [Hono](#hono) | `20809` | `5900` | `30731` |
| **24%** | [Koa](#koa) | `17888` | `5513` | `45032` |
| **11%** | [Carbon](#carbon) | `8337` | `1474` | `10450` |
| **9%** | [Express](#express) | `6533` | `1069` | `8444` |


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
    Reqs/sec      8594.56    3141.51   40155.88
    Latency        5.81ms     4.14ms   365.45ms
    HTTP codes:
      1xx - 0, 2xx - 96065, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3935
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3935
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
    Reqs/sec      6481.15     992.69    8562.60
    Latency        7.71ms     3.58ms   342.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24826.89    7231.37   36587.03
    Latency        2.01ms     1.88ms   170.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.64MB/s
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
    Reqs/sec     20213.23    5454.34   29580.85
    Latency        2.47ms     1.87ms   171.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     62811.62    3790.73   67261.81
    Latency      794.05us    69.91us     4.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     17779.51    6670.17   60347.53
    Latency        2.77ms     2.37ms   205.69ms
    HTTP codes:
      1xx - 0, 2xx - 91818, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8182
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8182
    Throughput:     3.74MB/s
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
    Reqs/sec     26120.96    8602.84   56876.43
    Latency        1.91ms     1.85ms   159.17ms
    HTTP codes:
      1xx - 0, 2xx - 96777, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3223
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3223
    Throughput:     5.79MB/s
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
    Reqs/sec     74800.27    5991.62   90541.29
    Latency      665.91us   168.48us     7.70ms
    HTTP codes:
      1xx - 0, 2xx - 96976, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3024
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3024
    Throughput:    11.48MB/s
  ```


