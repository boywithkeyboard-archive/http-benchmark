## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74455` | `5904` | `78758` |
| **86%** | [Hyper Express](#hyper-express) | `63874` | `3220` | `66967` |
| **31%** | [Node (Default)](#node-default) | `23237` | `6151` | `63023` |
| **29%** | [Fastify](#fastify) | `21239` | `5332` | `35653` |
| **25%** | [Hono](#hono) | `18392` | `4133` | `29584` |
| **23%** | [Koa](#koa) | `17420` | `5483` | `58318` |
| **11%** | [Carbon](#carbon) | `8249` | `1399` | `10649` |
| **9%** | [Express](#express) | `6450` | `986` | `8499` |


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
    Reqs/sec      8737.82    5009.23   60157.91
    Latency        5.71ms     4.19ms   364.04ms
    HTTP codes:
      1xx - 0, 2xx - 92172, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7828
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7828
    Throughput:     1.83MB/s
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
    Reqs/sec      6487.01    1020.44    8509.57
    Latency        7.70ms     3.58ms   341.54ms
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
    Reqs/sec     21413.72    4817.91   34583.17
    Latency        2.33ms     1.88ms   172.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     19165.24    4408.33   30321.94
    Latency        2.61ms     1.94ms   178.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.33MB/s
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
    Reqs/sec     64062.16    4221.42   78057.41
    Latency      778.45us    76.36us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.10MB/s
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
    Reqs/sec     16544.65    6079.30   62664.65
    Latency        3.01ms     2.35ms   203.23ms
    HTTP codes:
      1xx - 0, 2xx - 93578, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6422
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6422
    Throughput:     3.50MB/s
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
    Reqs/sec     24428.08    6836.01   68144.56
    Latency        2.04ms     1.78ms   150.68ms
    HTTP codes:
      1xx - 0, 2xx - 95767, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4233
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4233
    Throughput:     5.36MB/s
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
    Reqs/sec     73514.83    5597.22   79872.73
    Latency      677.92us   147.22us    11.36ms
    HTTP codes:
      1xx - 0, 2xx - 98214, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1786
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1786
    Throughput:    11.42MB/s
  ```


