## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79004` | `2904` | `86049` |
| **88%** | [Hyper Express](#hyper-express) | `69412` | `2905` | `74070` |
| **45%** | [Node (Default)](#node-default) | `35373` | `9779` | `86110` |
| **44%** | [Fastify](#fastify) | `34486` | `10359` | `51502` |
| **38%** | [Koa](#koa) | `29996` | `12138` | `81046` |
| **38%** | [Hono](#hono) | `29866` | `7983` | `48573` |
| **12%** | [Carbon](#carbon) | `9236` | `2203` | `13445` |
| **10%** | [Express](#express) | `7806` | `1763` | `10627` |


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
    Reqs/sec     10753.23    8447.47   79440.23
    Latency        4.64ms     4.35ms   372.77ms
    HTTP codes:
      1xx - 0, 2xx - 87909, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12091
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12091
    Throughput:     2.15MB/s
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
    Reqs/sec      8757.86    8895.59   89014.56
    Latency        5.70ms     3.79ms   343.11ms
    HTTP codes:
      1xx - 0, 2xx - 85431, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14569
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14562
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 7
    Throughput:     2.14MB/s
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
    Reqs/sec     35845.75   11448.30   51467.79
    Latency        1.39ms     1.95ms   170.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.13MB/s
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
    Reqs/sec     30784.72    9028.80   47258.64
    Latency        1.62ms     2.09ms   179.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.96MB/s
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
    Reqs/sec     69144.34    3759.27   74317.61
    Latency      721.63us    88.81us     3.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.82MB/s
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
    Reqs/sec     29289.72   11685.84   74911.20
    Latency        1.70ms     2.30ms   199.66ms
    HTTP codes:
      1xx - 0, 2xx - 92511, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7489
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7489
    Throughput:     6.12MB/s
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
    Reqs/sec     36075.63    8842.11   70102.30
    Latency        1.38ms     1.72ms   145.57ms
    HTTP codes:
      1xx - 0, 2xx - 97122, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2878
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2878
    Throughput:     8.02MB/s
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
    Reqs/sec     77888.62    2552.93   82787.60
    Latency      638.90us   190.23us    10.24ms
    HTTP codes:
      1xx - 0, 2xx - 96490, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3510
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3510
    Throughput:    11.89MB/s
  ```


