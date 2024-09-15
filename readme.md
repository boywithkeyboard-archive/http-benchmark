## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74588` | `5715` | `90356` |
| **87%** | [Hyper Express](#hyper-express) | `64954` | `5313` | `87529` |
| **35%** | [Node (Default)](#node-default) | `25830` | `7978` | `47193` |
| **32%** | [Fastify](#fastify) | `23925` | `6831` | `36446` |
| **28%** | [Hono](#hono) | `20961` | `5892` | `30785` |
| **23%** | [Koa](#koa) | `17098` | `6582` | `61844` |
| **11%** | [Carbon](#carbon) | `8273` | `1393` | `10487` |
| **9%** | [Express](#express) | `6362` | `958` | `8484` |


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
    Reqs/sec      8948.33    5046.61   60667.67
    Latency        5.59ms     4.18ms   358.21ms
    HTTP codes:
      1xx - 0, 2xx - 92210, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7790
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7790
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
    Reqs/sec      6634.87    1101.65    8411.10
    Latency        7.53ms     3.63ms   348.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     25045.73    7764.17   36451.94
    Latency        2.00ms     1.95ms   176.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.68MB/s
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
    Reqs/sec     20272.69    5370.52   29900.29
    Latency        2.46ms     2.00ms   178.70ms
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
    Reqs/sec     64022.90    3467.30   68727.25
    Latency      779.02us    70.45us     3.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     18066.02    6583.14   55318.99
    Latency        2.76ms     2.41ms   206.96ms
    HTTP codes:
      1xx - 0, 2xx - 93977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6023
    Throughput:     3.84MB/s
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
    Reqs/sec     25131.74    7258.40   43395.20
    Latency        1.99ms     1.91ms   165.03ms
    HTTP codes:
      1xx - 0, 2xx - 98262, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1738
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1738
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
    Reqs/sec     74137.81    6053.42   82899.51
    Latency      672.21us   178.52us    10.87ms
    HTTP codes:
      1xx - 0, 2xx - 97092, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2908
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2908
    Throughput:    11.38MB/s
  ```


