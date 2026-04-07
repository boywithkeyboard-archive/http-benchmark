## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75044` | `5136` | `95070` |
| **86%** | [Hyper Express](#hyper-express) | `64768` | `4221` | `72077` |
| **33%** | [Fastify](#fastify) | `24504` | `7593` | `36219` |
| **32%** | [Node (Default)](#node-default) | `24023` | `7440` | `74051` |
| **30%** | [Hono](#hono) | `22418` | `6845` | `31071` |
| **26%** | [Koa](#koa) | `19767` | `9516` | `80545` |
| **10%** | [Carbon](#carbon) | `7786` | `1336` | `10448` |
| **9%** | [Express](#express) | `6518` | `1134` | `8391` |


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
    Reqs/sec      8564.60    5935.34   69494.71
    Latency        5.82ms     4.34ms   373.77ms
    HTTP codes:
      1xx - 0, 2xx - 91021, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8979
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8979
    Throughput:     1.77MB/s
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
    Reqs/sec      6404.60    1111.75    8719.59
    Latency        7.80ms     3.69ms   351.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24166.44    8257.24   35686.39
    Latency        2.07ms     2.12ms   190.92ms
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
    Reqs/sec     23591.43    6854.82   30697.94
    Latency        2.12ms     2.08ms   185.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     64130.01    4196.40   76980.20
    Latency      777.58us    79.98us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     19403.72    9855.83   76682.15
    Latency        2.57ms     2.44ms   214.24ms
    HTTP codes:
      1xx - 0, 2xx - 90686, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9314
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9314
    Throughput:     3.98MB/s
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
    Reqs/sec     22762.94    6636.99   63277.28
    Latency        2.19ms     1.91ms   164.11ms
    HTTP codes:
      1xx - 0, 2xx - 97228, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2772
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2772
    Throughput:     5.07MB/s
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
    Reqs/sec     74598.05    3443.59   83747.00
    Latency      667.31us   201.29us    12.29ms
    HTTP codes:
      1xx - 0, 2xx - 96013, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3987
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3987
    Throughput:    11.34MB/s
  ```


