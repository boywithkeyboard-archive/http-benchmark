## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68779` | `3829` | `80068` |
| **85%** | [Hyper Express](#hyper-express) | `58398` | `3839` | `66734` |
| **32%** | [Hono](#hono) | `22015` | `7042` | `30463` |
| **29%** | [Node (Default)](#node-default) | `20141` | `5150` | `62705` |
| **28%** | [Fastify](#fastify) | `19177` | `4204` | `35149` |
| **27%** | [Koa](#koa) | `18609` | `9575` | `73863` |
| **11%** | [Carbon](#carbon) | `7464` | `1287` | `10343` |
| **9%** | [Express](#express) | `6140` | `1098` | `8327` |


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
    Reqs/sec      8075.58    7060.90   77623.53
    Latency        6.19ms     4.71ms   396.93ms
    HTTP codes:
      1xx - 0, 2xx - 88299, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11701
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11701
    Throughput:     1.62MB/s
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
    Reqs/sec      6178.31    1142.89    8311.46
    Latency        8.09ms     3.80ms   362.36ms
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
    Reqs/sec     19777.31    4321.72   35720.19
    Latency        2.53ms     2.07ms   188.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.49MB/s
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
    Reqs/sec     21433.17    6965.21   30764.66
    Latency        2.33ms     2.13ms   188.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     58161.48    3715.82   68142.82
    Latency        0.86ms    97.98us     4.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.26MB/s
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
    Reqs/sec     19001.97    8738.38   73611.10
    Latency        2.62ms     2.47ms   216.94ms
    HTTP codes:
      1xx - 0, 2xx - 91971, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8029
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8029
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
    Reqs/sec     19731.47    5090.89   65707.23
    Latency        2.53ms     2.11ms   179.23ms
    HTTP codes:
      1xx - 0, 2xx - 96610, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3390
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3390
    Throughput:     4.37MB/s
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
    Reqs/sec     69993.69    4015.70   83605.05
    Latency      710.61us   170.38us     7.93ms
    HTTP codes:
      1xx - 0, 2xx - 96606, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3394
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3394
    Throughput:    10.71MB/s
  ```


