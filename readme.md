## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69223` | `4091` | `77057` |
| **85%** | [Hyper Express](#hyper-express) | `58588` | `5378` | `70998` |
| **35%** | [Node (Default)](#node-default) | `24194` | `6554` | `65455` |
| **33%** | [Fastify](#fastify) | `23153` | `7146` | `36675` |
| **31%** | [Hono](#hono) | `21259` | `6190` | `30765` |
| **28%** | [Koa](#koa) | `19110` | `7992` | `64912` |
| **12%** | [Carbon](#carbon) | `8094` | `1461` | `10262` |
| **9%** | [Express](#express) | `6383` | `1099` | `8355` |


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
    Reqs/sec      8649.79    5242.27   62855.38
    Latency        5.77ms     4.33ms   374.09ms
    HTTP codes:
      1xx - 0, 2xx - 92727, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7273
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7273
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
    Reqs/sec      6411.13    1103.15    8314.40
    Latency        7.80ms     3.77ms   358.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23061.52    6857.48   36124.85
    Latency        2.17ms     2.06ms   184.18ms
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
    Reqs/sec     20955.18    6134.14   30409.90
    Latency        2.38ms     2.10ms   188.42ms
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
    Reqs/sec     58370.96    3282.25   67121.30
    Latency        0.85ms    76.59us     2.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.29MB/s
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
    Reqs/sec     19014.86    9000.62   70876.99
    Latency        2.62ms     2.29ms   201.02ms
    HTTP codes:
      1xx - 0, 2xx - 90974, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9026
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9026
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
    Reqs/sec     23462.62    6622.08   68537.99
    Latency        2.13ms     1.94ms   170.06ms
    HTTP codes:
      1xx - 0, 2xx - 96706, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3294
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3294
    Throughput:     5.20MB/s
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
    Reqs/sec     69730.94    2576.21   76277.93
    Latency      713.63us   174.17us    10.21ms
    HTTP codes:
      1xx - 0, 2xx - 95774, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4226
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4226
    Throughput:    10.57MB/s
  ```


