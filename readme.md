## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74384` | `2804` | `86956` |
| **85%** | [Hyper Express](#hyper-express) | `63090` | `3496` | `67459` |
| **35%** | [Node (Default)](#node-default) | `25796` | `8353` | `69657` |
| **34%** | [Fastify](#fastify) | `24938` | `7463` | `35815` |
| **28%** | [Hono](#hono) | `21076` | `5923` | `30193` |
| **26%** | [Koa](#koa) | `19397` | `9171` | `87037` |
| **11%** | [Carbon](#carbon) | `8330` | `1523` | `10543` |
| **9%** | [Express](#express) | `6462` | `1106` | `8406` |


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
    Reqs/sec      9038.03    6015.28   74328.71
    Latency        5.52ms     4.27ms   367.61ms
    HTTP codes:
      1xx - 0, 2xx - 91921, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8079
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8079
    Throughput:     1.89MB/s
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
    Reqs/sec      6912.07    5520.45   66882.65
    Latency        7.22ms     3.86ms   354.43ms
    HTTP codes:
      1xx - 0, 2xx - 90904, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9096
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9096
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
    Reqs/sec     25389.03    8166.77   37531.78
    Latency        1.97ms     1.93ms   175.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.76MB/s
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
    Reqs/sec     20297.06    5334.08   30289.13
    Latency        2.46ms     1.97ms   179.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     62958.89    3450.23   65779.58
    Latency      791.70us    64.52us     2.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18950.98    8009.42   66088.64
    Latency        2.63ms     2.47ms   215.42ms
    HTTP codes:
      1xx - 0, 2xx - 93029, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6971
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6971
    Throughput:     3.99MB/s
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
    Reqs/sec     25434.85    7603.68   61568.38
    Latency        1.96ms     1.86ms   159.00ms
    HTTP codes:
      1xx - 0, 2xx - 97513, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2487
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2487
    Throughput:     5.69MB/s
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
    Reqs/sec     75686.40    2510.90   83649.84
    Latency      657.63us   178.95us    10.96ms
    HTTP codes:
      1xx - 0, 2xx - 96278, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3722
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3722
    Throughput:    11.53MB/s
  ```


