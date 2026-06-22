## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72902` | `3921` | `88147` |
| **82%** | [Hyper Express](#hyper-express) | `59883` | `3615` | `66496` |
| **29%** | [Hono](#hono) | `21291` | `6089` | `30209` |
| **29%** | [Fastify](#fastify) | `21056` | `5503` | `35042` |
| **28%** | [Node (Default)](#node-default) | `20621` | `5350` | `62790` |
| **24%** | [Koa](#koa) | `17856` | `8127` | `66251` |
| **11%** | [Carbon](#carbon) | `7664` | `1370` | `10570` |
| **9%** | [Express](#express) | `6248` | `1107` | `8390` |


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
    Reqs/sec      7930.20    5338.82   72421.29
    Latency        6.29ms     4.41ms   375.22ms
    HTTP codes:
      1xx - 0, 2xx - 92695, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7305
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7305
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
    Reqs/sec      6251.14    1107.68    8437.22
    Latency        7.99ms     3.89ms   369.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     20993.89    5761.66   36271.34
    Latency        2.38ms     2.11ms   190.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     21720.89    6543.61   31392.40
    Latency        2.30ms     2.06ms   187.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     60278.41    4315.75   69106.38
    Latency      827.28us   102.29us     4.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.56MB/s
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
    Reqs/sec     18857.54    8771.63   66480.45
    Latency        2.64ms     2.46ms   212.97ms
    HTTP codes:
      1xx - 0, 2xx - 92632, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7368
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7368
    Throughput:     3.95MB/s
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
    Reqs/sec     20938.50    5555.66   74188.54
    Latency        2.38ms     1.92ms   162.29ms
    HTTP codes:
      1xx - 0, 2xx - 96493, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3507
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3507
    Throughput:     4.63MB/s
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
    Reqs/sec     72043.54    4707.36   83995.66
    Latency      690.98us   168.49us     8.10ms
    HTTP codes:
      1xx - 0, 2xx - 95123, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4877
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4877
    Throughput:    10.85MB/s
  ```


