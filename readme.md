## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68712` | `4208` | `81378` |
| **85%** | [Hyper Express](#hyper-express) | `58356` | `3831` | `72743` |
| **34%** | [Node (Default)](#node-default) | `23217` | `8505` | `71048` |
| **33%** | [Fastify](#fastify) | `22670` | `7031` | `35237` |
| **25%** | [Hono](#hono) | `17504` | `4837` | `27765` |
| **25%** | [Koa](#koa) | `16908` | `9596` | `76030` |
| **11%** | [Carbon](#carbon) | `7660` | `1312` | `10337` |
| **9%** | [Express](#express) | `6177` | `1024` | `8201` |


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
    Reqs/sec      8319.00    5132.49   68852.05
    Latency        6.00ms     4.50ms   378.62ms
    HTTP codes:
      1xx - 0, 2xx - 92609, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7391
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7391
    Throughput:     1.75MB/s
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
    Reqs/sec      6190.09    1054.23    8397.56
    Latency        8.07ms     3.80ms   362.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     24230.00    7604.80   33741.42
    Latency        2.06ms     2.07ms   187.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     18315.63    5750.89   28788.73
    Latency        2.73ms     2.20ms   194.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.14MB/s
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
    Reqs/sec     57610.17    4048.93   68389.05
    Latency        0.87ms   141.43us     5.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.18MB/s
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
    Reqs/sec     15928.45    7318.94   67474.19
    Latency        3.13ms     2.45ms   213.09ms
    HTTP codes:
      1xx - 0, 2xx - 92502, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7498
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7498
    Throughput:     3.34MB/s
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
    Reqs/sec     24805.21    8199.79   55422.89
    Latency        2.01ms     2.02ms   175.11ms
    HTTP codes:
      1xx - 0, 2xx - 97599, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2401
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2401
    Throughput:     5.54MB/s
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
    Reqs/sec     70195.34    5613.39   94454.91
    Latency      708.72us   223.03us    13.09ms
    HTTP codes:
      1xx - 0, 2xx - 95892, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4108
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4108
    Throughput:    10.65MB/s
  ```


