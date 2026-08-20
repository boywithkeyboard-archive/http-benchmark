## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79371` | `2495` | `83725` |
| **87%** | [Hyper Express](#hyper-express) | `69423` | `3395` | `72209` |
| **45%** | [Node (Default)](#node-default) | `35695` | `9097` | `67822` |
| **42%** | [Fastify](#fastify) | `33116` | `8780` | `51311` |
| **39%** | [Koa](#koa) | `31093` | `12980` | `77807` |
| **39%** | [Hono](#hono) | `30583` | `9467` | `49338` |
| **12%** | [Carbon](#carbon) | `9398` | `2330` | `13639` |
| **10%** | [Express](#express) | `7690` | `1784` | `10783` |


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
    Reqs/sec      9974.74    6904.97   77989.66
    Latency        5.00ms     4.44ms   375.01ms
    HTTP codes:
      1xx - 0, 2xx - 91334, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8666
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8666
    Throughput:     2.07MB/s
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
    Reqs/sec      8568.05    7803.04   85084.24
    Latency        5.82ms     3.85ms   347.90ms
    HTTP codes:
      1xx - 0, 2xx - 87864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12136
    Throughput:     2.16MB/s
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
    Reqs/sec     33615.40    9094.77   52354.48
    Latency        1.49ms     1.87ms   163.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.63MB/s
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
    Reqs/sec     30002.99    8375.26   47997.34
    Latency        1.67ms     2.14ms   181.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.78MB/s
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
    Reqs/sec     69722.73    2319.65   72122.36
    Latency      715.29us    66.98us     3.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.90MB/s
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
    Reqs/sec     30697.43   14452.00   89446.63
    Latency        1.63ms     2.21ms   190.32ms
    HTTP codes:
      1xx - 0, 2xx - 89382, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10618
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10618
    Throughput:     6.20MB/s
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
    Reqs/sec     35394.03    9152.49   76087.35
    Latency        1.41ms     1.65ms   137.52ms
    HTTP codes:
      1xx - 0, 2xx - 96640, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3360
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3360
    Throughput:     7.83MB/s
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
    Reqs/sec     79010.44    3846.21   89408.36
    Latency      630.55us   170.99us     9.52ms
    HTTP codes:
      1xx - 0, 2xx - 96484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3516
    Throughput:    12.05MB/s
  ```


