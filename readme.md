## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76146` | `2640` | `80914` |
| **82%** | [Hyper Express](#hyper-express) | `62781` | `3936` | `66492` |
| **34%** | [Node (Default)](#node-default) | `25804` | `8649` | `80668` |
| **32%** | [Fastify](#fastify) | `24130` | `7285` | `35596` |
| **28%** | [Hono](#hono) | `21144` | `5937` | `29444` |
| **24%** | [Koa](#koa) | `18453` | `8520` | `79398` |
| **11%** | [Carbon](#carbon) | `8088` | `1395` | `10400` |
| **8%** | [Express](#express) | `6284` | `986` | `8288` |


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
    Reqs/sec      9262.84    6704.90   81139.10
    Latency        5.39ms     4.28ms   366.18ms
    HTTP codes:
      1xx - 0, 2xx - 90139, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9861
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9861
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
    Reqs/sec      7127.19    6430.10   76428.13
    Latency        7.01ms     3.91ms   360.36ms
    HTTP codes:
      1xx - 0, 2xx - 88904, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11096
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11096
    Throughput:     1.81MB/s
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
    Reqs/sec     24155.91    7540.14   35742.48
    Latency        2.07ms     2.13ms   189.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20967.65    6280.38   29956.30
    Latency        2.38ms     2.05ms   182.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     63697.27    3732.93   73632.22
    Latency      782.69us    71.54us     3.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18942.84    8846.94   78224.96
    Latency        2.63ms     2.33ms   205.16ms
    HTTP codes:
      1xx - 0, 2xx - 91999, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8001
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8001
    Throughput:     3.94MB/s
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
    Reqs/sec     25422.38    8252.17   66018.17
    Latency        1.96ms     1.89ms   167.25ms
    HTTP codes:
      1xx - 0, 2xx - 96884, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3116
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3116
    Throughput:     5.64MB/s
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
    Reqs/sec     76351.33    2190.75   85106.84
    Latency      651.98us   169.19us     8.47ms
    HTTP codes:
      1xx - 0, 2xx - 94811, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5189
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5189
    Throughput:    11.46MB/s
  ```


