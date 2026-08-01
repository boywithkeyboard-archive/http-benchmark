## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71010` | `4572` | `79048` |
| **84%** | [Hyper Express](#hyper-express) | `59868` | `6381` | `96439` |
| **31%** | [Node (Default)](#node-default) | `22078` | `6720` | `69479` |
| **31%** | [Fastify](#fastify) | `21967` | `5647` | `36029` |
| **31%** | [Hono](#hono) | `21905` | `6491` | `29894` |
| **26%** | [Koa](#koa) | `18369` | `7257` | `62249` |
| **11%** | [Carbon](#carbon) | `7551` | `1285` | `10482` |
| **9%** | [Express](#express) | `6299` | `1111` | `8311` |


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
    Reqs/sec      8284.42    5834.88   68038.46
    Latency        6.02ms     4.76ms   400.36ms
    HTTP codes:
      1xx - 0, 2xx - 91073, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8927
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8927
    Throughput:     1.71MB/s
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
    Reqs/sec      6182.32    1022.97    8290.25
    Latency        8.08ms     3.75ms   360.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     22553.10    6904.69   35783.43
    Latency        2.21ms     2.11ms   187.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.12MB/s
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
    Reqs/sec     23329.30    6912.34   29797.96
    Latency        2.14ms     2.28ms   199.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.28MB/s
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
    Reqs/sec     60035.99    3198.70   66226.33
    Latency      830.52us    91.26us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.53MB/s
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
    Reqs/sec     20228.27    7847.14   59882.06
    Latency        2.47ms     2.47ms   213.51ms
    HTTP codes:
      1xx - 0, 2xx - 93886, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6114
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6114
    Throughput:     4.30MB/s
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
    Reqs/sec     21017.17    5940.48   71120.97
    Latency        2.37ms     1.88ms   162.56ms
    HTTP codes:
      1xx - 0, 2xx - 96543, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3457
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3457
    Throughput:     4.65MB/s
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
    Reqs/sec     70575.92    4708.73   83778.80
    Latency      706.22us   151.89us    10.59ms
    HTTP codes:
      1xx - 0, 2xx - 97596, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2404
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2404
    Throughput:    10.89MB/s
  ```


