## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72633` | `5804` | `80263` |
| **84%** | [Hyper Express](#hyper-express) | `61191` | `6109` | `66592` |
| **35%** | [Node (Default)](#node-default) | `25368` | `7641` | `44525` |
| **34%** | [Fastify](#fastify) | `24537` | `7694` | `35970` |
| **28%** | [Hono](#hono) | `20009` | `5441` | `28852` |
| **25%** | [Koa](#koa) | `18154` | `5647` | `54369` |
| **11%** | [Carbon](#carbon) | `8229` | `1429` | `10364` |
| **9%** | [Express](#express) | `6266` | `991` | `8353` |


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
    Reqs/sec      8707.33    3733.86   56589.06
    Latency        5.73ms     4.22ms   367.47ms
    HTTP codes:
      1xx - 0, 2xx - 95016, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4984
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4984
    Throughput:     1.88MB/s
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
    Reqs/sec      6580.07    1063.59    8386.75
    Latency        7.60ms     3.65ms   348.50ms
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
    Reqs/sec     24827.56    7958.83   37131.73
    Latency        2.01ms     1.93ms   177.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     20029.34    5479.12   29923.17
    Latency        2.49ms     2.08ms   188.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     61459.15    3476.04   63779.39
    Latency      810.62us    67.00us     4.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.74MB/s
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
    Reqs/sec     19034.58    7068.51   55217.65
    Latency        2.62ms     2.40ms   209.63ms
    HTTP codes:
      1xx - 0, 2xx - 92951, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7049
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7049
    Throughput:     4.00MB/s
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
    Reqs/sec     26021.37    7782.40   57206.37
    Latency        1.91ms     1.91ms   161.11ms
    HTTP codes:
      1xx - 0, 2xx - 97091, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2909
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2909
    Throughput:     5.80MB/s
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
    Reqs/sec     73080.29    6006.28   81347.44
    Latency      681.43us   180.70us     8.87ms
    HTTP codes:
      1xx - 0, 2xx - 98317, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1683
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1683
    Throughput:    11.36MB/s
  ```


