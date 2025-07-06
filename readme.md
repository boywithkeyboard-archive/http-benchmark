## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75856` | `5527` | `84148` |
| **81%** | [Hyper Express](#hyper-express) | `61545` | `3033` | `65905` |
| **34%** | [Node (Default)](#node-default) | `25751` | `7540` | `56264` |
| **31%** | [Fastify](#fastify) | `23741` | `7248` | `35888` |
| **27%** | [Hono](#hono) | `20652` | `5930` | `29360` |
| **24%** | [Koa](#koa) | `18086` | `6686` | `53628` |
| **10%** | [Carbon](#carbon) | `7952` | `1324` | `10075` |
| **8%** | [Express](#express) | `6268` | `1011` | `8332` |


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
    Reqs/sec      8759.83    4312.57   52894.41
    Latency        5.69ms     4.36ms   375.01ms
    HTTP codes:
      1xx - 0, 2xx - 93622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6378
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
    Reqs/sec      6321.58    1032.73    8285.90
    Latency        7.91ms     3.57ms   345.86ms
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
    Reqs/sec     23711.48    6991.03   34807.98
    Latency        2.11ms     2.01ms   181.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     20962.17    5727.46   29950.93
    Latency        2.38ms     2.09ms   186.58ms
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
    Reqs/sec     61173.41    3306.78   63456.67
    Latency      814.93us    74.84us     3.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.69MB/s
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
    Reqs/sec     17760.70    6459.95   51223.27
    Latency        2.81ms     2.35ms   208.89ms
    HTTP codes:
      1xx - 0, 2xx - 94443, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5557
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5557
    Throughput:     3.80MB/s
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
    Reqs/sec     25011.93    6847.04   48977.99
    Latency        2.00ms     1.86ms   162.16ms
    HTTP codes:
      1xx - 0, 2xx - 97714, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2286
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2286
    Throughput:     5.60MB/s
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
    Reqs/sec     73648.42    5267.87   80813.60
    Latency      676.46us   163.46us     6.42ms
    HTTP codes:
      1xx - 0, 2xx - 97582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2418
    Throughput:    11.37MB/s
  ```


