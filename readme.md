## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73015` | `4180` | `78818` |
| **84%** | [Hyper Express](#hyper-express) | `61190` | `4130` | `83805` |
| **35%** | [Node (Default)](#node-default) | `25553` | `8151` | `58233` |
| **32%** | [Fastify](#fastify) | `23431` | `6784` | `35865` |
| **28%** | [Hono](#hono) | `20125` | `5714` | `29661` |
| **26%** | [Koa](#koa) | `19020` | `7122` | `53302` |
| **11%** | [Carbon](#carbon) | `8195` | `1431` | `10356` |
| **9%** | [Express](#express) | `6493` | `1093` | `8378` |


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
    Reqs/sec      8432.73    3412.12   49829.99
    Latency        5.92ms     4.30ms   376.81ms
    HTTP codes:
      1xx - 0, 2xx - 95426, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4574
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4574
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
    Reqs/sec      6339.17    1004.77    8312.21
    Latency        7.89ms     3.59ms   345.33ms
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
    Reqs/sec     23320.56    6853.95   36797.54
    Latency        2.14ms     1.96ms   178.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20969.26    6188.44   30375.07
    Latency        2.38ms     2.07ms   185.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     61121.81    3513.37   70016.44
    Latency      816.11us    76.19us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     19406.26    7039.20   54542.08
    Latency        2.57ms     2.47ms   215.46ms
    HTTP codes:
      1xx - 0, 2xx - 94100, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5900
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5900
    Throughput:     4.13MB/s
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
    Reqs/sec     25276.76    7351.04   54709.62
    Latency        1.98ms     1.88ms   164.83ms
    HTTP codes:
      1xx - 0, 2xx - 97599, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2401
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2401
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
    Reqs/sec     73002.44    6608.52   83358.41
    Latency      682.24us   274.23us    18.75ms
    HTTP codes:
      1xx - 0, 2xx - 96226, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3774
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3774
    Throughput:    11.11MB/s
  ```


