## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74375` | `2793` | `80554` |
| **84%** | [Hyper Express](#hyper-express) | `62821` | `3384` | `66601` |
| **34%** | [Node (Default)](#node-default) | `25278` | `8399` | `85569` |
| **33%** | [Fastify](#fastify) | `24212` | `7452` | `34989` |
| **28%** | [Hono](#hono) | `20663` | `5797` | `28770` |
| **26%** | [Koa](#koa) | `19304` | `9644` | `80012` |
| **11%** | [Carbon](#carbon) | `8233` | `1493` | `10289` |
| **9%** | [Express](#express) | `6453` | `1101` | `8372` |


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
    Reqs/sec      8819.72    5085.30   64325.88
    Latency        5.66ms     4.39ms   379.61ms
    HTTP codes:
      1xx - 0, 2xx - 93556, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6444
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6444
    Throughput:     1.87MB/s
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
    Reqs/sec      7074.37    5761.73   66948.63
    Latency        7.06ms     3.99ms   362.45ms
    HTTP codes:
      1xx - 0, 2xx - 90427, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9573
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9573
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
    Reqs/sec     23398.84    6806.49   35312.42
    Latency        2.14ms     2.00ms   180.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     20774.76    5393.42   29690.73
    Latency        2.41ms     2.04ms   182.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     64942.57    3938.59   67489.35
    Latency      767.53us    68.61us     2.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.23MB/s
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
    Reqs/sec     18677.00    8798.11   73505.51
    Latency        2.67ms     2.51ms   216.54ms
    HTTP codes:
      1xx - 0, 2xx - 91190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8810
    Throughput:     3.85MB/s
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
    Reqs/sec     25295.21    8053.40   80158.60
    Latency        1.97ms     2.00ms   168.37ms
    HTTP codes:
      1xx - 0, 2xx - 96669, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3331
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3331
    Throughput:     5.61MB/s
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
    Reqs/sec     75108.86    2763.68   83926.52
    Latency      663.49us   148.60us     4.96ms
    HTTP codes:
      1xx - 0, 2xx - 97305, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2695
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2695
    Throughput:    11.57MB/s
  ```


