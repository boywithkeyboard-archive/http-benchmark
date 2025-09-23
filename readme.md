## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73789` | `2683` | `78254` |
| **85%** | [Hyper Express](#hyper-express) | `62618` | `3563` | `67270` |
| **34%** | [Node (Default)](#node-default) | `24953` | `7556` | `76674` |
| **30%** | [Fastify](#fastify) | `22336` | `6346` | `35094` |
| **27%** | [Hono](#hono) | `19773` | `5620` | `29443` |
| **25%** | [Koa](#koa) | `18520` | `8300` | `76555` |
| **11%** | [Carbon](#carbon) | `8224` | `1464` | `10427` |
| **9%** | [Express](#express) | `6322` | `1076` | `8353` |


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
    Reqs/sec      9164.45    7910.26   81723.82
    Latency        5.43ms     4.48ms   380.69ms
    HTTP codes:
      1xx - 0, 2xx - 87625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12375
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
    Reqs/sec      6885.77    5495.83   68228.55
    Latency        7.25ms     4.06ms   372.94ms
    HTTP codes:
      1xx - 0, 2xx - 90852, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9148
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9148
    Throughput:     1.79MB/s
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
    Reqs/sec     22210.69    6436.04   34534.29
    Latency        2.25ms     2.08ms   186.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.03MB/s
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
    Reqs/sec     20677.21    5875.34   29768.61
    Latency        2.42ms     2.00ms   179.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     61464.68    3976.57   65722.67
    Latency      811.77us    81.13us     2.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     19296.83    9807.71   81335.02
    Latency        2.59ms     2.38ms   208.01ms
    HTTP codes:
      1xx - 0, 2xx - 89609, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10391
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10391
    Throughput:     3.91MB/s
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
    Reqs/sec     25294.46    8266.04   65521.33
    Latency        1.97ms     1.99ms   169.99ms
    HTTP codes:
      1xx - 0, 2xx - 97112, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2888
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2888
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
    Reqs/sec     73847.30    2658.92   81918.72
    Latency      673.98us   159.14us     8.55ms
    HTTP codes:
      1xx - 0, 2xx - 96557, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3443
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3443
    Throughput:    11.29MB/s
  ```


