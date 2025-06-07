## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72306` | `4591` | `77829` |
| **85%** | [Hyper Express](#hyper-express) | `61604` | `3452` | `63713` |
| **34%** | [Node (Default)](#node-default) | `24905` | `7055` | `57279` |
| **33%** | [Fastify](#fastify) | `24009` | `7311` | `36760` |
| **28%** | [Hono](#hono) | `20494` | `5877` | `29749` |
| **26%** | [Koa](#koa) | `18635` | `7478` | `56859` |
| **11%** | [Carbon](#carbon) | `8191` | `1432` | `10331` |
| **9%** | [Express](#express) | `6349` | `1069` | `8261` |


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
    Reqs/sec      8505.74    4666.37   50592.35
    Latency        5.87ms     4.33ms   377.10ms
    HTTP codes:
      1xx - 0, 2xx - 92826, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7174
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7174
    Throughput:     1.79MB/s
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
    Reqs/sec      6298.50    1079.75    8371.44
    Latency        7.93ms     4.01ms   379.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23925.67    7701.96   36453.18
    Latency        2.09ms     2.05ms   185.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     21543.29    6432.83   30549.86
    Latency        2.32ms     2.08ms   187.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     61572.82    3409.59   65698.48
    Latency      810.30us    78.60us     3.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.74MB/s
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
    Reqs/sec     18233.89    7026.66   59247.63
    Latency        2.73ms     2.42ms   212.50ms
    HTTP codes:
      1xx - 0, 2xx - 93338, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6662
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6662
    Throughput:     3.86MB/s
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
    Reqs/sec     25458.05    7518.26   59163.83
    Latency        1.96ms     1.87ms   164.35ms
    HTTP codes:
      1xx - 0, 2xx - 96910, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3090
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3090
    Throughput:     5.65MB/s
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
    Reqs/sec     73142.67    4489.58   80607.44
    Latency      680.78us   151.96us     6.64ms
    HTTP codes:
      1xx - 0, 2xx - 96977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3023
    Throughput:    11.23MB/s
  ```


