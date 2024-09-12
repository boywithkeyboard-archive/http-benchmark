## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73306` | `5842` | `81258` |
| **85%** | [Hyper Express](#hyper-express) | `62638` | `4099` | `68759` |
| **36%** | [Node (Default)](#node-default) | `26119` | `7831` | `42430` |
| **33%** | [Fastify](#fastify) | `24276` | `6886` | `36904` |
| **28%** | [Hono](#hono) | `20790` | `5644` | `30098` |
| **25%** | [Koa](#koa) | `18155` | `6361` | `54912` |
| **12%** | [Carbon](#carbon) | `8435` | `1492` | `10820` |
| **9%** | [Express](#express) | `6624` | `1051` | `8513` |


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
    Reqs/sec      8639.51    3360.97   53862.63
    Latency        5.78ms     4.08ms   352.99ms
    HTTP codes:
      1xx - 0, 2xx - 95660, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4340
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4340
    Throughput:     1.88MB/s
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
    Reqs/sec      6525.18    1007.36    8591.04
    Latency        7.66ms     3.39ms   327.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     25617.65    8407.16   37036.17
    Latency        1.95ms     2.02ms   178.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.82MB/s
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
    Reqs/sec     21196.96    5732.74   29937.76
    Latency        2.36ms     1.96ms   178.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62509.68    3531.74   67242.79
    Latency      797.47us    66.19us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     17860.64    5749.41   46527.32
    Latency        2.79ms     2.28ms   199.68ms
    HTTP codes:
      1xx - 0, 2xx - 95550, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4450
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4450
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
    Reqs/sec     27846.11    8602.73   49558.20
    Latency        1.79ms     1.85ms   159.12ms
    HTTP codes:
      1xx - 0, 2xx - 97543, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2457
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2457
    Throughput:     6.21MB/s
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
    Reqs/sec     73537.55    5656.80   81932.99
    Latency      677.31us   234.19us    16.34ms
    HTTP codes:
      1xx - 0, 2xx - 95833, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4167
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4167
    Throughput:    11.15MB/s
  ```


