## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72279` | `4542` | `80557` |
| **84%** | [Hyper Express](#hyper-express) | `60403` | `3367` | `63169` |
| **35%** | [Node (Default)](#node-default) | `25432` | `8157` | `64562` |
| **34%** | [Fastify](#fastify) | `24544` | `7542` | `37230` |
| **27%** | [Hono](#hono) | `19841` | `5525` | `29085` |
| **26%** | [Koa](#koa) | `18914` | `6482` | `53249` |
| **11%** | [Carbon](#carbon) | `8235` | `1384` | `10296` |
| **9%** | [Express](#express) | `6363` | `1034` | `8375` |


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
    Reqs/sec      8786.40    4666.06   57011.02
    Latency        5.68ms     4.31ms   371.12ms
    HTTP codes:
      1xx - 0, 2xx - 93120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6880
    Throughput:     1.86MB/s
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
    Reqs/sec      6473.69    1029.88    8431.91
    Latency        7.72ms     3.63ms   346.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     23510.73    6964.13   35353.89
    Latency        2.12ms     2.09ms   185.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     20104.67    5626.32   29749.98
    Latency        2.49ms     2.01ms   183.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.54MB/s
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
    Reqs/sec     59970.62    3970.29   64674.65
    Latency      830.86us    98.46us     4.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.52MB/s
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
    Reqs/sec     18886.49    6771.46   53903.49
    Latency        2.64ms     2.48ms   216.80ms
    HTTP codes:
      1xx - 0, 2xx - 93549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6451
    Throughput:     4.00MB/s
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
    Reqs/sec     25717.69    8025.54   60360.21
    Latency        1.94ms     2.00ms   169.96ms
    HTTP codes:
      1xx - 0, 2xx - 96618, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3382
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3382
    Throughput:     5.68MB/s
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
    Reqs/sec     73992.96    5809.72   88467.73
    Latency      673.63us   157.83us     6.25ms
    HTTP codes:
      1xx - 0, 2xx - 97148, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2852
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2852
    Throughput:    11.37MB/s
  ```


