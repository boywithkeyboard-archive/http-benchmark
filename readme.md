## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69264` | `3647` | `83427` |
| **85%** | [Hyper Express](#hyper-express) | `58795` | `3241` | `65389` |
| **32%** | [Hono](#hono) | `21987` | `7088` | `31314` |
| **29%** | [Fastify](#fastify) | `20299` | `4807` | `36675` |
| **29%** | [Node (Default)](#node-default) | `20262` | `5291` | `67235` |
| **27%** | [Koa](#koa) | `19030` | `9233` | `77099` |
| **11%** | [Carbon](#carbon) | `7523` | `1283` | `10460` |
| **9%** | [Express](#express) | `6296` | `1172` | `8424` |


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
    Reqs/sec      8080.21    4514.26   65976.15
    Latency        6.18ms     4.63ms   390.87ms
    HTTP codes:
      1xx - 0, 2xx - 93800, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6200
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6200
    Throughput:     1.72MB/s
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
    Reqs/sec      6338.39    1151.08    8461.43
    Latency        7.88ms     3.88ms   368.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     20663.27    5788.28   36799.02
    Latency        2.42ms     2.08ms   183.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     21550.79    6589.35   31291.93
    Latency        2.32ms     1.93ms   179.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     58654.77    3827.14   67462.75
    Latency        0.85ms   104.80us     4.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.33MB/s
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
    Reqs/sec     20019.24    9163.67   74446.07
    Latency        2.49ms     2.48ms   218.64ms
    HTTP codes:
      1xx - 0, 2xx - 91895, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8105
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8105
    Throughput:     4.16MB/s
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
    Reqs/sec     20007.62    5319.06   66201.91
    Latency        2.49ms     1.85ms   164.60ms
    HTTP codes:
      1xx - 0, 2xx - 96455, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3545
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3545
    Throughput:     4.42MB/s
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
    Reqs/sec     69444.88    3883.68   78463.40
    Latency      717.03us   170.77us     9.33ms
    HTTP codes:
      1xx - 0, 2xx - 96799, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3201
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3201
    Throughput:    10.63MB/s
  ```


