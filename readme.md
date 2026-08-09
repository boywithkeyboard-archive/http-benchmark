## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78448` | `3092` | `84805` |
| **88%** | [Hyper Express](#hyper-express) | `68725` | `2823` | `73587` |
| **46%** | [Node (Default)](#node-default) | `36327` | `10337` | `79867` |
| **42%** | [Fastify](#fastify) | `32943` | `8434` | `50402` |
| **39%** | [Hono](#hono) | `30415` | `8372` | `47658` |
| **38%** | [Koa](#koa) | `29453` | `11482` | `74315` |
| **12%** | [Carbon](#carbon) | `9195` | `2126` | `13581` |
| **10%** | [Express](#express) | `7631` | `1750` | `10617` |


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
    Reqs/sec     10043.10    7008.43   80773.27
    Latency        4.97ms     4.34ms   373.29ms
    HTTP codes:
      1xx - 0, 2xx - 90957, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9043
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9043
    Throughput:     2.08MB/s
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
    Reqs/sec      8084.02    6514.32   74433.13
    Latency        6.18ms     3.93ms   356.97ms
    HTTP codes:
      1xx - 0, 2xx - 90143, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9857
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9857
    Throughput:     2.09MB/s
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
    Reqs/sec     34784.01   10169.45   51950.24
    Latency        1.44ms     1.81ms   159.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.89MB/s
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
    Reqs/sec     29792.47    8322.52   47021.49
    Latency        1.68ms     1.95ms   173.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.73MB/s
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
    Reqs/sec     68793.67    2772.72   72203.71
    Latency      724.91us    72.78us     3.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.77MB/s
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
    Reqs/sec     30104.19   12324.50   72991.11
    Latency        1.65ms     2.32ms   199.69ms
    HTTP codes:
      1xx - 0, 2xx - 92274, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7726
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7726
    Throughput:     6.29MB/s
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
    Reqs/sec     35433.17   10906.24   84609.52
    Latency        1.41ms     1.91ms   158.50ms
    HTTP codes:
      1xx - 0, 2xx - 93827, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6173
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6173
    Throughput:     7.62MB/s
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
    Reqs/sec     78792.33    3008.52   83581.91
    Latency      632.42us   159.41us     6.91ms
    HTTP codes:
      1xx - 0, 2xx - 97157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2843
    Throughput:    12.12MB/s
  ```


