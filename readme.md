## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75168` | `5545` | `87886` |
| **83%** | [Hyper Express](#hyper-express) | `62468` | `3233` | `65567` |
| **36%** | [Node (Default)](#node-default) | `27024` | `8064` | `58172` |
| **34%** | [Fastify](#fastify) | `25521` | `7590` | `37825` |
| **28%** | [Hono](#hono) | `21248` | `6059` | `31870` |
| **25%** | [Koa](#koa) | `18520` | `6194` | `52684` |
| **11%** | [Carbon](#carbon) | `8574` | `1462` | `10757` |
| **9%** | [Express](#express) | `6574` | `1041` | `8526` |


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
    Reqs/sec      8881.40    4228.94   51579.70
    Latency        5.62ms     4.20ms   365.72ms
    HTTP codes:
      1xx - 0, 2xx - 94364, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5636
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5636
    Throughput:     1.90MB/s
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
    Reqs/sec      7307.78    4914.11   62854.40
    Latency        6.83ms     3.89ms   352.36ms
    HTTP codes:
      1xx - 0, 2xx - 91404, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8596
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8596
    Throughput:     1.91MB/s
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
    Reqs/sec     25345.86    7858.71   37197.69
    Latency        1.97ms     1.97ms   177.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.75MB/s
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
    Reqs/sec     21250.02    6227.84   31195.08
    Latency        2.35ms     1.93ms   173.59ms
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
    Reqs/sec     64070.70    4384.21   70502.92
    Latency      778.92us    73.96us     4.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     20610.84    7869.28   58664.47
    Latency        2.42ms     2.40ms   208.58ms
    HTTP codes:
      1xx - 0, 2xx - 92928, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7072
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7072
    Throughput:     4.33MB/s
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
    Reqs/sec     28322.44    9120.53   51783.12
    Latency        1.76ms     1.94ms   164.14ms
    HTTP codes:
      1xx - 0, 2xx - 97640, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2360
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2360
    Throughput:     6.33MB/s
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
    Reqs/sec     75144.23    5206.86   83938.70
    Latency      662.26us   204.78us    10.40ms
    HTTP codes:
      1xx - 0, 2xx - 95798, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4202
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4202
    Throughput:    11.39MB/s
  ```


