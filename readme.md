## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71482` | `3905` | `79565` |
| **83%** | [Hyper Express](#hyper-express) | `59336` | `4201` | `69920` |
| **32%** | [Node (Default)](#node-default) | `22977` | `6801` | `69033` |
| **30%** | [Fastify](#fastify) | `21491` | `5727` | `36747` |
| **30%** | [Hono](#hono) | `21202` | `6784` | `30831` |
| **28%** | [Koa](#koa) | `19679` | `8053` | `59703` |
| **11%** | [Carbon](#carbon) | `8009` | `1462` | `10254` |
| **8%** | [Express](#express) | `6039` | `1041` | `8180` |


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
    Reqs/sec      8795.90    6348.89   76879.13
    Latency        5.67ms     4.53ms   385.21ms
    HTTP codes:
      1xx - 0, 2xx - 89859, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10141
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10141
    Throughput:     1.80MB/s
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
    Reqs/sec      6298.73    1094.64    8304.77
    Latency        7.93ms     3.89ms   372.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21840.87    5847.98   36611.65
    Latency        2.29ms     2.11ms   188.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     21661.85    7017.03   31173.13
    Latency        2.31ms     2.17ms   195.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.89MB/s
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
    Reqs/sec     59564.68    3802.03   68155.79
    Latency      837.24us    86.45us     3.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.46MB/s
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
    Reqs/sec     19245.43    7675.40   56662.27
    Latency        2.59ms     2.53ms   224.53ms
    HTTP codes:
      1xx - 0, 2xx - 93250, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6750
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6750
    Throughput:     4.06MB/s
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
    Reqs/sec     23827.48    6668.60   65693.10
    Latency        2.09ms     2.01ms   177.19ms
    HTTP codes:
      1xx - 0, 2xx - 96400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3600
    Throughput:     5.26MB/s
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
    Reqs/sec     71255.10    3534.78   82408.25
    Latency      697.36us   166.66us     9.39ms
    HTTP codes:
      1xx - 0, 2xx - 96331, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3669
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3669
    Throughput:    10.88MB/s
  ```


