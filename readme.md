## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74682` | `5982` | `82906` |
| **87%** | [Hyper Express](#hyper-express) | `64646` | `3832` | `72654` |
| **34%** | [Node (Default)](#node-default) | `25445` | `7634` | `44858` |
| **31%** | [Fastify](#fastify) | `22946` | `6739` | `35991` |
| **28%** | [Hono](#hono) | `21220` | `6196` | `30380` |
| **24%** | [Koa](#koa) | `17799` | `5871` | `42670` |
| **11%** | [Carbon](#carbon) | `7925` | `1315` | `10220` |
| **9%** | [Express](#express) | `6492` | `1024` | `8528` |


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
    Reqs/sec      8362.96    3437.71   43536.04
    Latency        5.97ms     4.24ms   371.02ms
    HTTP codes:
      1xx - 0, 2xx - 95568, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4432
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4432
    Throughput:     1.82MB/s
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
    Reqs/sec      6490.03    1069.16    8521.51
    Latency        7.70ms     3.62ms   343.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     22985.14    6971.43   35959.96
    Latency        2.17ms     1.96ms   177.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.21MB/s
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
    Reqs/sec     20183.56    5534.58   29971.94
    Latency        2.47ms     1.91ms   173.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     64206.52    3516.31   67123.88
    Latency      776.76us    63.42us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.12MB/s
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
    Reqs/sec     18644.99    7224.06   67552.56
    Latency        2.67ms     2.29ms   199.38ms
    HTTP codes:
      1xx - 0, 2xx - 92437, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7563
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7563
    Throughput:     3.90MB/s
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
    Reqs/sec     25117.42    7323.76   54023.09
    Latency        1.99ms     1.94ms   166.40ms
    HTTP codes:
      1xx - 0, 2xx - 97696, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2304
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2304
    Throughput:     5.62MB/s
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
    Reqs/sec     73588.22    9052.54   91593.51
    Latency      677.08us   335.44us    19.39ms
    HTTP codes:
      1xx - 0, 2xx - 97713, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2287
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2287
    Throughput:    11.37MB/s
  ```


