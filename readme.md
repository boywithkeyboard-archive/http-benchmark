## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75309` | `2238` | `79403` |
| **83%** | [Hyper Express](#hyper-express) | `62433` | `2954` | `66333` |
| **34%** | [Node (Default)](#node-default) | `25555` | `8320` | `69730` |
| **30%** | [Fastify](#fastify) | `22786` | `6511` | `35331` |
| **28%** | [Hono](#hono) | `20824` | `6061` | `30976` |
| **25%** | [Koa](#koa) | `18975` | `9105` | `79078` |
| **11%** | [Carbon](#carbon) | `8338` | `1519` | `10413` |
| **9%** | [Express](#express) | `6482` | `1104` | `8441` |


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
    Reqs/sec      8841.07    5266.23   65775.66
    Latency        5.64ms     4.33ms   373.44ms
    HTTP codes:
      1xx - 0, 2xx - 92950, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7050
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7050
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
    Reqs/sec      6432.81    1021.55    8488.24
    Latency        7.77ms     3.60ms   345.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     24733.85    7453.79   37235.71
    Latency        2.02ms     1.88ms   169.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.60MB/s
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
    Reqs/sec     20715.74    5964.68   30371.28
    Latency        2.41ms     2.08ms   187.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62597.05    3627.17   65356.17
    Latency      796.75us    77.85us     2.96ms
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
    Reqs/sec     19300.87    8255.82   67677.54
    Latency        2.58ms     2.39ms   210.34ms
    HTTP codes:
      1xx - 0, 2xx - 93142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6858
    Throughput:     4.07MB/s
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
    Reqs/sec     25555.70    8360.45   68462.98
    Latency        1.95ms     1.88ms   161.96ms
    HTTP codes:
      1xx - 0, 2xx - 96271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3729
    Throughput:     5.63MB/s
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
    Reqs/sec     75070.82    2108.57   81703.96
    Latency      662.87us   193.92us    10.97ms
    HTTP codes:
      1xx - 0, 2xx - 96378, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3622
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3622
    Throughput:    11.45MB/s
  ```


