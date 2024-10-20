## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72384` | `5213` | `79354` |
| **86%** | [Hyper Express](#hyper-express) | `62064` | `3111` | `70248` |
| **36%** | [Node (Default)](#node-default) | `26005` | `7450` | `50242` |
| **34%** | [Fastify](#fastify) | `24563` | `7099` | `35803` |
| **28%** | [Hono](#hono) | `20304` | `5144` | `28856` |
| **25%** | [Koa](#koa) | `18022` | `6614` | `56116` |
| **11%** | [Carbon](#carbon) | `8217` | `1364` | `10516` |
| **9%** | [Express](#express) | `6511` | `1083` | `8574` |


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
    Reqs/sec      8428.58    3735.68   48459.08
    Latency        5.91ms     4.24ms   369.33ms
    HTTP codes:
      1xx - 0, 2xx - 94537, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5463
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5463
    Throughput:     1.81MB/s
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
    Reqs/sec      6725.64    1122.90    8585.72
    Latency        7.43ms     3.37ms   326.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.92MB/s
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
    Reqs/sec     23889.66    6912.56   35847.32
    Latency        2.09ms     2.01ms   180.60ms
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
    Reqs/sec     21220.39    5771.83   30134.95
    Latency        2.35ms     1.91ms   174.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62747.59    3985.03   74816.55
    Latency      794.21us    63.86us     2.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     18338.13    6017.65   56698.88
    Latency        2.72ms     2.23ms   187.73ms
    HTTP codes:
      1xx - 0, 2xx - 94666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5334
    Throughput:     3.92MB/s
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
    Reqs/sec     25897.13    7284.36   41676.05
    Latency        1.92ms     1.80ms   157.76ms
    HTTP codes:
      1xx - 0, 2xx - 98259, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1741
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1741
    Throughput:     5.85MB/s
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
    Reqs/sec     73463.05    6228.42   80320.46
    Latency      679.38us   173.91us    14.12ms
    HTTP codes:
      1xx - 0, 2xx - 98351, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1649
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1649
    Throughput:    11.42MB/s
  ```


