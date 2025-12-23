## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74593` | `3114` | `81647` |
| **83%** | [Hyper Express](#hyper-express) | `62029` | `3506` | `65533` |
| **34%** | [Node (Default)](#node-default) | `25674` | `8309` | `74757` |
| **31%** | [Fastify](#fastify) | `23198` | `7178` | `35306` |
| **27%** | [Hono](#hono) | `20427` | `5923` | `29736` |
| **25%** | [Koa](#koa) | `18303` | `8578` | `79033` |
| **11%** | [Carbon](#carbon) | `8240` | `1453` | `10432` |
| **9%** | [Express](#express) | `6411` | `1043` | `8278` |


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
    Reqs/sec      8906.05    6607.03   83457.67
    Latency        5.60ms     4.35ms   374.18ms
    HTTP codes:
      1xx - 0, 2xx - 90063, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9937
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9937
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
    Reqs/sec      6360.22    1037.25    8259.40
    Latency        7.86ms     3.71ms   354.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23048.42    6629.24   35413.74
    Latency        2.17ms     2.11ms   187.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     20869.73    6135.63   29535.97
    Latency        2.39ms     2.08ms   186.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62674.84    3930.51   65954.92
    Latency      795.64us    72.80us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18373.16    9184.68   79295.68
    Latency        2.71ms     2.36ms   209.01ms
    HTTP codes:
      1xx - 0, 2xx - 90676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9324
    Throughput:     3.77MB/s
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
    Reqs/sec     25317.25    7956.31   65235.29
    Latency        1.97ms     2.03ms   175.04ms
    HTTP codes:
      1xx - 0, 2xx - 97194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2806
    Throughput:     5.64MB/s
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
    Reqs/sec     74596.61    2567.21   80993.10
    Latency      668.59us   153.23us     7.68ms
    HTTP codes:
      1xx - 0, 2xx - 97462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2538
    Throughput:    11.49MB/s
  ```


