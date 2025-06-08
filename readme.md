## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73518` | `4819` | `79322` |
| **85%** | [Hyper Express](#hyper-express) | `62714` | `3602` | `68538` |
| **33%** | [Node (Default)](#node-default) | `24269` | `6596` | `46504` |
| **33%** | [Fastify](#fastify) | `24254` | `7568` | `37118` |
| **30%** | [Hono](#hono) | `21894` | `6537` | `30806` |
| **26%** | [Koa](#koa) | `19443` | `7648` | `58409` |
| **11%** | [Carbon](#carbon) | `8096` | `1435` | `10236` |
| **9%** | [Express](#express) | `6282` | `997` | `8470` |


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
    Reqs/sec      8435.40    4421.96   59161.79
    Latency        5.92ms     4.36ms   376.22ms
    HTTP codes:
      1xx - 0, 2xx - 93551, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6449
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6449
    Throughput:     1.79MB/s
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
    Reqs/sec      6362.25    1060.30    8292.25
    Latency        7.86ms     3.69ms   356.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23714.17    7271.02   37273.69
    Latency        2.11ms     2.05ms   181.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     20825.29    6039.20   30334.22
    Latency        2.40ms     2.06ms   186.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62045.61    4389.99   67943.36
    Latency      803.08us    99.59us     5.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18169.81    6233.61   45049.41
    Latency        2.75ms     2.47ms   217.74ms
    HTTP codes:
      1xx - 0, 2xx - 95238, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4762
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4762
    Throughput:     3.91MB/s
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
    Reqs/sec     25484.07    6382.78   49391.08
    Latency        1.96ms     1.90ms   164.44ms
    HTTP codes:
      1xx - 0, 2xx - 97853, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2147
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2147
    Throughput:     5.71MB/s
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
    Reqs/sec     72916.57    4984.41   84562.91
    Latency      683.41us   168.32us     8.23ms
    HTTP codes:
      1xx - 0, 2xx - 97535, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2465
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2465
    Throughput:    11.25MB/s
  ```


