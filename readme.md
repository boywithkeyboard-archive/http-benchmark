## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75596` | `2847` | `86174` |
| **83%** | [Hyper Express](#hyper-express) | `62835` | `4181` | `66569` |
| **33%** | [Node (Default)](#node-default) | `25101` | `7181` | `63551` |
| **32%** | [Fastify](#fastify) | `23841` | `7017` | `35401` |
| **27%** | [Hono](#hono) | `20339` | `5560` | `29298` |
| **26%** | [Koa](#koa) | `19451` | `7923` | `64616` |
| **10%** | [Carbon](#carbon) | `7864` | `1265` | `10307` |
| **9%** | [Express](#express) | `6470` | `1104` | `8378` |


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
    Reqs/sec      8796.03    6191.30   75501.03
    Latency        5.68ms     4.24ms   365.12ms
    HTTP codes:
      1xx - 0, 2xx - 91486, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8514
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8514
    Throughput:     1.83MB/s
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
    Reqs/sec      6968.63    5297.62   65656.62
    Latency        7.16ms     3.85ms   354.26ms
    HTTP codes:
      1xx - 0, 2xx - 91540, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8460
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8460
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
    Reqs/sec     23266.14    6746.03   35178.96
    Latency        2.15ms     2.10ms   188.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.28MB/s
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
    Reqs/sec     20520.14    5408.53   29142.71
    Latency        2.44ms     2.11ms   187.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     62568.47    3877.96   67518.78
    Latency      797.01us    78.38us     3.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18762.28    7876.40   66319.83
    Latency        2.66ms     2.41ms   214.33ms
    HTTP codes:
      1xx - 0, 2xx - 93221, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6779
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6779
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
    Reqs/sec     26094.61    8474.22   64832.30
    Latency        1.91ms     1.86ms   162.04ms
    HTTP codes:
      1xx - 0, 2xx - 97388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2612
    Throughput:     5.82MB/s
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
    Reqs/sec     73887.05    2832.84   81030.51
    Latency      673.54us   195.67us    11.21ms
    HTTP codes:
      1xx - 0, 2xx - 96531, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3469
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3469
    Throughput:    11.29MB/s
  ```


