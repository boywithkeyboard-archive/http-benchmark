## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69732` | `3898` | `78985` |
| **83%** | [Hyper Express](#hyper-express) | `57884` | `3915` | `63363` |
| **31%** | [Hono](#hono) | `21531` | `6896` | `30911` |
| **31%** | [Node (Default)](#node-default) | `21459` | `5223` | `57754` |
| **29%** | [Fastify](#fastify) | `20024` | `4541` | `36109` |
| **24%** | [Koa](#koa) | `16597` | `6574` | `57610` |
| **11%** | [Carbon](#carbon) | `7868` | `1523` | `10326` |
| **9%** | [Express](#express) | `6002` | `1033` | `8334` |


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
    Reqs/sec      8171.57    5165.46   72537.24
    Latency        6.11ms     4.46ms   385.16ms
    HTTP codes:
      1xx - 0, 2xx - 92671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7329
    Throughput:     1.72MB/s
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
    Reqs/sec      6144.10    1127.06    8462.32
    Latency        8.14ms     3.89ms   372.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     21200.96    5563.11   36744.44
    Latency        2.36ms     2.15ms   190.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     21106.00    6800.30   31247.07
    Latency        2.37ms     2.23ms   197.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     58885.13    3682.02   65782.25
    Latency      846.68us    91.76us     4.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.37MB/s
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
    Reqs/sec     18533.71    8793.47   73359.74
    Latency        2.69ms     2.47ms   218.25ms
    HTTP codes:
      1xx - 0, 2xx - 91608, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8392
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8392
    Throughput:     3.84MB/s
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
    Reqs/sec     22013.10    5971.24   69998.49
    Latency        2.27ms     2.06ms   176.79ms
    HTTP codes:
      1xx - 0, 2xx - 96599, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3401
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3401
    Throughput:     4.87MB/s
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
    Reqs/sec     69654.37    3178.30   76224.30
    Latency      714.86us   123.57us     4.33ms
    HTTP codes:
      1xx - 0, 2xx - 97617, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2383
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2383
    Throughput:    10.76MB/s
  ```


