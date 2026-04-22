## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71243` | `4752` | `83654` |
| **83%** | [Hyper Express](#hyper-express) | `59279` | `3555` | `66029` |
| **31%** | [Hono](#hono) | `21978` | `7023` | `31040` |
| **29%** | [Fastify](#fastify) | `20822` | `6043` | `36702` |
| **29%** | [Node (Default)](#node-default) | `20623` | `8241` | `85128` |
| **27%** | [Koa](#koa) | `19450` | `9951` | `80540` |
| **10%** | [Carbon](#carbon) | `7339` | `1190` | `10135` |
| **8%** | [Express](#express) | `6046` | `1081` | `8182` |


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
    Reqs/sec      8036.13    5796.49   67818.92
    Latency        6.21ms     4.41ms   378.50ms
    HTTP codes:
      1xx - 0, 2xx - 90788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9212
    Throughput:     1.66MB/s
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
    Reqs/sec      6123.24    1056.58    8321.58
    Latency        8.16ms     4.01ms   382.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20797.95    5688.84   36415.83
    Latency        2.40ms     2.10ms   186.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     21264.49    6718.34   31298.93
    Latency        2.35ms     2.03ms   186.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     59741.66    3660.19   71595.56
    Latency      834.50us    84.06us     4.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     17530.64    9737.79   82855.25
    Latency        2.84ms     2.61ms   228.63ms
    HTTP codes:
      1xx - 0, 2xx - 89331, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10669
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10669
    Throughput:     3.54MB/s
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
    Reqs/sec     20381.16    5071.01   64829.24
    Latency        2.45ms     1.93ms   163.98ms
    HTTP codes:
      1xx - 0, 2xx - 97124, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2876
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2876
    Throughput:     4.53MB/s
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
    Reqs/sec     71275.84    3951.76   83432.34
    Latency      698.70us   189.66us     9.06ms
    HTTP codes:
      1xx - 0, 2xx - 96829, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3171
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3171
    Throughput:    10.92MB/s
  ```


