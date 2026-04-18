## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75389` | `4544` | `84395` |
| **82%** | [Hyper Express](#hyper-express) | `61948` | `3305` | `68556` |
| **30%** | [Hono](#hono) | `22895` | `6228` | `30668` |
| **30%** | [Node (Default)](#node-default) | `22667` | `6388` | `65681` |
| **29%** | [Fastify](#fastify) | `21884` | `5354` | `38640` |
| **25%** | [Koa](#koa) | `19010` | `8804` | `69671` |
| **10%** | [Carbon](#carbon) | `7518` | `1130` | `10451` |
| **9%** | [Express](#express) | `6464` | `1068` | `8354` |


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
    Reqs/sec      8359.63    6250.58   80302.91
    Latency        5.97ms     4.45ms   377.68ms
    HTTP codes:
      1xx - 0, 2xx - 90857, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9143
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9143
    Throughput:     1.73MB/s
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
    Reqs/sec      6498.98    1103.97    8351.79
    Latency        7.69ms     3.69ms   354.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     22915.01    6463.00   36744.39
    Latency        2.18ms     2.01ms   181.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.19MB/s
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
    Reqs/sec     23208.07    6478.45   30908.93
    Latency        2.15ms     2.03ms   181.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     61632.63    3712.62   69531.88
    Latency      809.95us    85.92us     4.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     19902.41    8661.93   65762.23
    Latency        2.51ms     2.43ms   212.09ms
    HTTP codes:
      1xx - 0, 2xx - 92823, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7177
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7177
    Throughput:     4.17MB/s
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
    Reqs/sec     23209.32    6855.24   69299.74
    Latency        2.15ms     2.03ms   176.99ms
    HTTP codes:
      1xx - 0, 2xx - 96444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3556
    Throughput:     5.13MB/s
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
    Reqs/sec     74960.52    4975.67   89113.29
    Latency      664.67us   143.55us     8.15ms
    HTTP codes:
      1xx - 0, 2xx - 97484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2516
    Throughput:    11.57MB/s
  ```


