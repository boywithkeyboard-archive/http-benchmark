## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70652` | `4061` | `80756` |
| **82%** | [Hyper Express](#hyper-express) | `57666` | `3446` | `67126` |
| **31%** | [Hono](#hono) | `22147` | `7339` | `31874` |
| **30%** | [Fastify](#fastify) | `21419` | `6366` | `35925` |
| **28%** | [Node (Default)](#node-default) | `19942` | `5248` | `64783` |
| **26%** | [Koa](#koa) | `18262` | `10125` | `80554` |
| **10%** | [Carbon](#carbon) | `7409` | `1270` | `10314` |
| **9%** | [Express](#express) | `6138` | `1106` | `8247` |


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
    Reqs/sec      7855.27    5505.84   73484.93
    Latency        6.35ms     4.44ms   378.77ms
    HTTP codes:
      1xx - 0, 2xx - 91916, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8084
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8084
    Throughput:     1.64MB/s
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
    Reqs/sec      6138.15    1072.05    8200.18
    Latency        8.14ms     3.95ms   377.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     21041.11    5763.19   36011.04
    Latency        2.37ms     2.12ms   188.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     20471.90    6310.44   31239.73
    Latency        2.44ms     2.04ms   186.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     59272.19    3584.87   77302.64
    Latency      841.72us    88.96us     3.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.42MB/s
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
    Reqs/sec     18374.53    9154.37   78339.70
    Latency        2.72ms     2.62ms   224.32ms
    HTTP codes:
      1xx - 0, 2xx - 91671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8329
    Throughput:     3.81MB/s
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
    Reqs/sec     20718.00    7089.68   77498.09
    Latency        2.41ms     1.97ms   174.75ms
    HTTP codes:
      1xx - 0, 2xx - 93814, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6186
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6186
    Throughput:     4.45MB/s
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
    Reqs/sec     70421.63    4203.50   80225.57
    Latency      707.24us   165.66us     7.43ms
    HTTP codes:
      1xx - 0, 2xx - 96933, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3067
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3067
    Throughput:    10.80MB/s
  ```


