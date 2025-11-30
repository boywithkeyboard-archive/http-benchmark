## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73093` | `2639` | `79611` |
| **82%** | [Hyper Express](#hyper-express) | `59895` | `3775` | `63659` |
| **33%** | [Node (Default)](#node-default) | `23816` | `7075` | `65997` |
| **31%** | [Fastify](#fastify) | `22995` | `7420` | `35904` |
| **27%** | [Hono](#hono) | `19462` | `5777` | `28532` |
| **24%** | [Koa](#koa) | `17858` | `8523` | `72107` |
| **11%** | [Carbon](#carbon) | `8039` | `1482` | `10303` |
| **9%** | [Express](#express) | `6336` | `1120` | `8215` |


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
    Reqs/sec      8639.47    5891.69   74088.69
    Latency        5.77ms     4.37ms   374.75ms
    HTTP codes:
      1xx - 0, 2xx - 91689, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8311
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8311
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
    Reqs/sec      6244.55    1082.59    8195.54
    Latency        8.00ms     3.87ms   367.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23257.54    7721.57   36600.40
    Latency        2.15ms     2.14ms   188.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.27MB/s
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
    Reqs/sec     20599.75    6438.71   30464.77
    Latency        2.42ms     2.08ms   187.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     59739.00    3270.03   63021.78
    Latency      834.85us    79.42us     3.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     17974.10    9323.44   76390.33
    Latency        2.78ms     2.45ms   215.13ms
    HTTP codes:
      1xx - 0, 2xx - 90800, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9200
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9200
    Throughput:     3.69MB/s
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
    Reqs/sec     24834.33    7774.74   57528.90
    Latency        2.01ms     2.00ms   178.97ms
    HTTP codes:
      1xx - 0, 2xx - 96798, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3202
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3202
    Throughput:     5.50MB/s
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
    Reqs/sec     73875.64    3357.25   86251.30
    Latency      674.42us   199.60us    10.08ms
    HTTP codes:
      1xx - 0, 2xx - 96547, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3453
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3453
    Throughput:    11.29MB/s
  ```


