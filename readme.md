## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72046` | `5556` | `79155` |
| **85%** | [Hyper Express](#hyper-express) | `61270` | `3779` | `66336` |
| **31%** | [Fastify](#fastify) | `22230` | `6481` | `37018` |
| **31%** | [Hono](#hono) | `22051` | `7286` | `31557` |
| **28%** | [Node (Default)](#node-default) | `20530` | `6174` | `73473` |
| **24%** | [Koa](#koa) | `17455` | `8206` | `66772` |
| **10%** | [Carbon](#carbon) | `7309` | `1140` | `10491` |
| **9%** | [Express](#express) | `6180` | `1108` | `8491` |


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
    Reqs/sec      7937.21    5430.10   71911.25
    Latency        6.29ms     4.41ms   374.66ms
    HTTP codes:
      1xx - 0, 2xx - 92511, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7489
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7489
    Throughput:     1.67MB/s
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
    Reqs/sec      6140.30    1046.36    8341.81
    Latency        8.14ms     3.92ms   373.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     20567.25    5344.99   39156.09
    Latency        2.43ms     2.06ms   181.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     21669.06    6208.38   31143.57
    Latency        2.30ms     1.98ms   177.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     61738.21    3352.44   66063.96
    Latency      807.60us    78.95us     2.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     19270.98    9301.82   67067.00
    Latency        2.59ms     2.47ms   215.21ms
    HTTP codes:
      1xx - 0, 2xx - 92344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7656
    Throughput:     4.03MB/s
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
    Reqs/sec     20306.31    5453.67   68133.47
    Latency        2.46ms     2.04ms   177.66ms
    HTTP codes:
      1xx - 0, 2xx - 97150, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2850
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2850
    Throughput:     4.52MB/s
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
    Reqs/sec     74116.01    4256.51   84073.39
    Latency      671.32us   180.09us    10.44ms
    HTTP codes:
      1xx - 0, 2xx - 95278, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4722
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4722
    Throughput:    11.18MB/s
  ```


