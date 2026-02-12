## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75996` | `2731` | `83238` |
| **85%** | [Hyper Express](#hyper-express) | `64279` | `3260` | `67825` |
| **33%** | [Node (Default)](#node-default) | `25319` | `8034` | `80005` |
| **33%** | [Fastify](#fastify) | `24780` | `7503` | `36183` |
| **28%** | [Hono](#hono) | `21244` | `6046` | `30668` |
| **26%** | [Koa](#koa) | `19585` | `9076` | `78254` |
| **11%** | [Carbon](#carbon) | `8496` | `1527` | `10470` |
| **9%** | [Express](#express) | `6501` | `1074` | `8512` |


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
    Reqs/sec      8906.07    5219.51   58279.02
    Latency        5.60ms     4.35ms   374.84ms
    HTTP codes:
      1xx - 0, 2xx - 91484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8516
    Throughput:     1.85MB/s
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
    Reqs/sec      7236.05    5901.73   79175.85
    Latency        6.90ms     3.80ms   348.86ms
    HTTP codes:
      1xx - 0, 2xx - 90518, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9482
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9482
    Throughput:     1.88MB/s
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
    Reqs/sec     25149.83    8011.84   37083.23
    Latency        1.99ms     2.04ms   183.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     21507.71    6052.96   30767.31
    Latency        2.32ms     1.94ms   177.23ms
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
    Reqs/sec     63813.36    3789.69   67226.48
    Latency      780.85us    73.06us     2.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     19157.22    8693.56   70423.67
    Latency        2.60ms     2.39ms   213.91ms
    HTTP codes:
      1xx - 0, 2xx - 91353, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8647
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8647
    Throughput:     3.96MB/s
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
    Reqs/sec     26362.54    8254.83   67104.44
    Latency        1.89ms     1.93ms   165.81ms
    HTTP codes:
      1xx - 0, 2xx - 97192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2808
    Throughput:     5.87MB/s
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
    Reqs/sec     75707.66    2854.19   85883.12
    Latency      658.13us   142.32us     5.61ms
    HTTP codes:
      1xx - 0, 2xx - 97396, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2604
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2604
    Throughput:    11.67MB/s
  ```


