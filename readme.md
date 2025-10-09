## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74593` | `2902` | `81591` |
| **82%** | [Hyper Express](#hyper-express) | `60858` | `3755` | `64961` |
| **34%** | [Node (Default)](#node-default) | `25656` | `8220` | `70263` |
| **31%** | [Fastify](#fastify) | `23224` | `6538` | `36876` |
| **29%** | [Hono](#hono) | `21573` | `6426` | `31107` |
| **25%** | [Koa](#koa) | `18849` | `8939` | `79702` |
| **11%** | [Carbon](#carbon) | `8299` | `1424` | `10463` |
| **8%** | [Express](#express) | `6331` | `1056` | `8257` |


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
    Reqs/sec      8981.26    5997.53   74917.61
    Latency        5.55ms     4.28ms   369.91ms
    HTTP codes:
      1xx - 0, 2xx - 91428, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8572
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8572
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
    Reqs/sec      6898.22    5503.19   71683.23
    Latency        7.24ms     3.97ms   364.09ms
    HTTP codes:
      1xx - 0, 2xx - 90879, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9121
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9121
    Throughput:     1.80MB/s
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
    Reqs/sec     23804.15    7255.23   36397.88
    Latency        2.10ms     2.02ms   179.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     21991.26    6230.74   30715.72
    Latency        2.27ms     1.93ms   176.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.97MB/s
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
    Reqs/sec     62105.97    3692.84   64882.73
    Latency      802.67us    74.26us     3.01ms
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
    Reqs/sec     19142.81    9527.11   80289.15
    Latency        2.61ms     2.39ms   208.71ms
    HTTP codes:
      1xx - 0, 2xx - 90668, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9332
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9332
    Throughput:     3.93MB/s
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
    Reqs/sec     25674.54    8346.23   60796.12
    Latency        1.94ms     1.89ms   165.60ms
    HTTP codes:
      1xx - 0, 2xx - 97270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2730
    Throughput:     5.72MB/s
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
    Reqs/sec     75114.30    3237.43   87554.17
    Latency      662.82us   177.23us    10.27ms
    HTTP codes:
      1xx - 0, 2xx - 96335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3665
    Throughput:    11.44MB/s
  ```


