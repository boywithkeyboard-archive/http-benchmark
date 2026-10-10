## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `102949` | `3196` | `106579` |
| **87%** | [Hyper Express](#hyper-express) | `89380` | `5731` | `92440` |
| **47%** | [Node (Default)](#node-default) | `47948` | `14434` | `93596` |
| **42%** | [Fastify](#fastify) | `43015` | `12963` | `66523` |
| **37%** | [Hono](#hono) | `38035` | `11142` | `59723` |
| **36%** | [Koa](#koa) | `37232` | `15646` | `105693` |
| **12%** | [Carbon](#carbon) | `11949` | `2840` | `17174` |
| **9%** | [Express](#express) | `9514` | `2034` | `13454` |


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
    Reqs/sec     12882.62    8324.91   91837.86
    Latency        3.88ms     3.37ms   292.85ms
    HTTP codes:
      1xx - 0, 2xx - 92114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7886
    Throughput:     2.69MB/s
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
    Reqs/sec     10730.64   10179.36   99903.14
    Latency        4.65ms     3.02ms   275.53ms
    HTTP codes:
      1xx - 0, 2xx - 87114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12886
    Throughput:     2.68MB/s
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
    Reqs/sec     42632.25   12229.91   67320.41
    Latency        1.17ms     1.58ms   136.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.67MB/s
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
    Reqs/sec     38939.21   11926.55   61237.39
    Latency        1.28ms     1.48ms   130.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     90575.11    4030.10   95702.86
    Latency      550.88us    45.25us     2.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.86MB/s
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
    Reqs/sec     40798.58   17612.41  115962.90
    Latency        1.22ms     1.70ms   141.01ms
    HTTP codes:
      1xx - 0, 2xx - 87937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12063
    Throughput:     8.12MB/s
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
    Reqs/sec     48889.17   13573.43   83493.55
    Latency        1.02ms     1.38ms   118.18ms
    HTTP codes:
      1xx - 0, 2xx - 96848, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3152
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3152
    Throughput:    10.82MB/s
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
    Reqs/sec    102805.11    4075.84  112242.20
    Latency      483.14us   170.47us     9.38ms
    HTTP codes:
      1xx - 0, 2xx - 94780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5220
    Throughput:    15.43MB/s
  ```


