## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72672` | `4720` | `92904` |
| **84%** | [Hyper Express](#hyper-express) | `60987` | `3416` | `64021` |
| **35%** | [Node (Default)](#node-default) | `25424` | `6684` | `44583` |
| **33%** | [Fastify](#fastify) | `24113` | `7225` | `35640` |
| **28%** | [Hono](#hono) | `20217` | `5420` | `29101` |
| **26%** | [Koa](#koa) | `18783` | `7308` | `56646` |
| **11%** | [Carbon](#carbon) | `8118` | `1366` | `10254` |
| **9%** | [Express](#express) | `6404` | `1036` | `8355` |


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
    Reqs/sec      8863.14    5472.16   64673.61
    Latency        5.63ms     4.25ms   371.74ms
    HTTP codes:
      1xx - 0, 2xx - 91097, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8903
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8903
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
    Reqs/sec      6557.26    1084.03    8398.39
    Latency        7.62ms     3.58ms   342.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23386.30    6790.48   36429.89
    Latency        2.14ms     2.05ms   182.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.31MB/s
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
    Reqs/sec     20802.09    5719.26   30105.51
    Latency        2.40ms     2.09ms   187.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     62017.72    3494.21   65015.15
    Latency      804.06us    69.34us     2.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     19545.72    7661.96   59605.98
    Latency        2.56ms     2.29ms   202.71ms
    HTTP codes:
      1xx - 0, 2xx - 91756, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8244
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8244
    Throughput:     4.05MB/s
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
    Reqs/sec     26483.76    8287.54   59513.27
    Latency        1.89ms     1.97ms   168.09ms
    HTTP codes:
      1xx - 0, 2xx - 97203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2797
    Throughput:     5.89MB/s
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
    Reqs/sec     72643.18    4357.66   80711.95
    Latency      685.77us   155.58us     6.95ms
    HTTP codes:
      1xx - 0, 2xx - 97649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2351
    Throughput:    11.22MB/s
  ```


