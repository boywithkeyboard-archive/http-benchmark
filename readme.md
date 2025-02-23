## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72861` | `4480` | `82075` |
| **87%** | [Hyper Express](#hyper-express) | `63375` | `4935` | `75718` |
| **35%** | [Node (Default)](#node-default) | `25830` | `6838` | `47805` |
| **32%** | [Fastify](#fastify) | `23591` | `7020` | `36141` |
| **28%** | [Hono](#hono) | `20611` | `5513` | `29186` |
| **25%** | [Koa](#koa) | `18188` | `6201` | `55463` |
| **11%** | [Carbon](#carbon) | `8273` | `1421` | `10247` |
| **9%** | [Express](#express) | `6388` | `1006` | `8501` |


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
    Reqs/sec      8631.86    3770.38   48913.06
    Latency        5.78ms     4.19ms   365.28ms
    HTTP codes:
      1xx - 0, 2xx - 95339, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4661
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4661
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
    Reqs/sec      6437.29    1026.82    8472.55
    Latency        7.77ms     3.71ms   354.46ms
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
    Reqs/sec     24497.47    7618.76   36478.29
    Latency        2.04ms     2.00ms   181.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     21227.63    5935.21   30320.70
    Latency        2.35ms     1.98ms   179.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62733.85    3631.47   68363.05
    Latency      795.03us    66.46us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     18697.62    8091.81   65460.04
    Latency        2.67ms     2.24ms   195.67ms
    HTTP codes:
      1xx - 0, 2xx - 90541, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9459
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9459
    Throughput:     3.83MB/s
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
    Reqs/sec     25425.17    6818.64   49527.38
    Latency        1.96ms     1.72ms   149.36ms
    HTTP codes:
      1xx - 0, 2xx - 97887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2113
    Throughput:     5.70MB/s
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
    Reqs/sec     74472.35    5231.73   84676.40
    Latency      669.54us   178.80us     7.89ms
    HTTP codes:
      1xx - 0, 2xx - 97198, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2802
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2802
    Throughput:    11.45MB/s
  ```


