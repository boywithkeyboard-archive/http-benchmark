## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79607` | `3743` | `94323` |
| **88%** | [Hyper Express](#hyper-express) | `69928` | `3468` | `75402` |
| **45%** | [Node (Default)](#node-default) | `35868` | `10116` | `78355` |
| **41%** | [Fastify](#fastify) | `32551` | `8964` | `51237` |
| **37%** | [Koa](#koa) | `29798` | `13844` | `84469` |
| **36%** | [Hono](#hono) | `28803` | `8766` | `48394` |
| **12%** | [Carbon](#carbon) | `9255` | `2392` | `13466` |
| **9%** | [Express](#express) | `7307` | `1639` | `10251` |


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
    Reqs/sec      9974.48    6546.64   76815.88
    Latency        5.00ms     4.23ms   367.11ms
    HTTP codes:
      1xx - 0, 2xx - 92144, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7856
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7856
    Throughput:     2.09MB/s
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
    Reqs/sec      8099.82    7472.26   79945.31
    Latency        6.16ms     3.87ms   351.74ms
    HTTP codes:
      1xx - 0, 2xx - 88431, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11569
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11569
    Throughput:     2.05MB/s
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
    Reqs/sec     34021.16   10426.28   52002.65
    Latency        1.47ms     1.97ms   170.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.70MB/s
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
    Reqs/sec     30561.44    9491.01   46616.41
    Latency        1.63ms     2.05ms   180.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.91MB/s
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
    Reqs/sec     70089.51    3425.43   72558.28
    Latency      711.55us    73.98us     3.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.95MB/s
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
    Reqs/sec     30705.48   13586.79   86801.19
    Latency        1.63ms     2.28ms   196.04ms
    HTTP codes:
      1xx - 0, 2xx - 90953, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9047
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9047
    Throughput:     6.30MB/s
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
    Reqs/sec     34560.40    9416.29   72089.78
    Latency        1.44ms     1.74ms   154.11ms
    HTTP codes:
      1xx - 0, 2xx - 96723, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3277
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3277
    Throughput:     7.65MB/s
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
    Reqs/sec     79611.07    3509.36   84913.99
    Latency      625.51us   177.70us    10.37ms
    HTTP codes:
      1xx - 0, 2xx - 95607, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4393
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4393
    Throughput:    12.05MB/s
  ```


