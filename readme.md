## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75155` | `2960` | `84830` |
| **84%** | [Hyper Express](#hyper-express) | `63342` | `3692` | `68373` |
| **36%** | [Node (Default)](#node-default) | `26932` | `8887` | `80284` |
| **32%** | [Fastify](#fastify) | `24198` | `7252` | `37470` |
| **28%** | [Hono](#hono) | `21311` | `5914` | `30669` |
| **26%** | [Koa](#koa) | `19275` | `8018` | `67855` |
| **11%** | [Carbon](#carbon) | `8302` | `1462` | `10573` |
| **9%** | [Express](#express) | `6446` | `1074` | `8336` |


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
    Reqs/sec      8924.44    6228.60   78711.92
    Latency        5.59ms     4.29ms   370.92ms
    HTTP codes:
      1xx - 0, 2xx - 91339, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8661
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8661
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
    Reqs/sec      7107.84    5674.29   67892.24
    Latency        7.02ms     3.95ms   364.89ms
    HTTP codes:
      1xx - 0, 2xx - 90752, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9248
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9248
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
    Reqs/sec     23886.24    7083.31   35990.60
    Latency        2.09ms     2.06ms   183.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     21018.96    5721.68   30103.59
    Latency        2.38ms     2.02ms   181.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     64169.38    4335.25   68275.05
    Latency      776.99us    70.20us     2.99ms
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
    Reqs/sec     19265.36    8997.20   80429.16
    Latency        2.59ms     2.50ms   217.37ms
    HTTP codes:
      1xx - 0, 2xx - 91463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8537
    Throughput:     3.98MB/s
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
    Reqs/sec     26529.82    8374.19   76975.90
    Latency        1.88ms     1.90ms   160.32ms
    HTTP codes:
      1xx - 0, 2xx - 96378, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3622
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3622
    Throughput:     5.85MB/s
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
    Reqs/sec     74249.19    2507.57   82175.33
    Latency      671.18us   141.02us     6.27ms
    HTTP codes:
      1xx - 0, 2xx - 97465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2535
    Throughput:    11.45MB/s
  ```


