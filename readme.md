## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70364` | `4049` | `84819` |
| **82%** | [Hyper Express](#hyper-express) | `57494` | `3667` | `68436` |
| **30%** | [Fastify](#fastify) | `20973` | `5298` | `36425` |
| **30%** | [Node (Default)](#node-default) | `20855` | `7519` | `77916` |
| **29%** | [Hono](#hono) | `20263` | `6362` | `29421` |
| **28%** | [Koa](#koa) | `19577` | `9522` | `75116` |
| **10%** | [Carbon](#carbon) | `7346` | `1197` | `11393` |
| **9%** | [Express](#express) | `6212` | `1113` | `8519` |


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
    Reqs/sec      7882.46    5524.74   61376.38
    Latency        6.33ms     4.67ms   396.20ms
    HTTP codes:
      1xx - 0, 2xx - 91278, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8722
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8722
    Throughput:     1.63MB/s
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
    Reqs/sec      6213.95    1110.19    8296.78
    Latency        8.04ms     3.85ms   366.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     21528.52    6430.63   36159.93
    Latency        2.32ms     2.15ms   192.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     20774.49    6104.98   28935.58
    Latency        2.40ms     2.31ms   203.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     57707.55    3493.08   64098.50
    Latency        0.86ms    94.48us     3.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.20MB/s
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
    Reqs/sec     17512.41    8004.34   62755.58
    Latency        2.85ms     2.48ms   218.86ms
    HTTP codes:
      1xx - 0, 2xx - 92080, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7920
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7920
    Throughput:     3.64MB/s
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
    Reqs/sec     20446.63    5372.26   59355.78
    Latency        2.44ms     2.13ms   184.73ms
    HTTP codes:
      1xx - 0, 2xx - 97243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2757
    Throughput:     4.55MB/s
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
    Reqs/sec     68520.05    4587.02   88611.41
    Latency      729.26us   248.09us    12.92ms
    HTTP codes:
      1xx - 0, 2xx - 95764, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4236
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4236
    Throughput:    10.35MB/s
  ```


