## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74997` | `2596` | `81088` |
| **85%** | [Hyper Express](#hyper-express) | `63623` | `4486` | `79509` |
| **36%** | [Node (Default)](#node-default) | `26721` | `9834` | `83041` |
| **31%** | [Fastify](#fastify) | `23589` | `6946` | `35890` |
| **28%** | [Hono](#hono) | `21029` | `5849` | `30902` |
| **25%** | [Koa](#koa) | `18559` | `8841` | `80841` |
| **11%** | [Carbon](#carbon) | `8362` | `1446` | `10429` |
| **9%** | [Express](#express) | `6506` | `1094` | `8474` |


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
    Reqs/sec      9014.51    5497.04   65984.11
    Latency        5.53ms     4.29ms   370.98ms
    HTTP codes:
      1xx - 0, 2xx - 92692, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7308
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7308
    Throughput:     1.90MB/s
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
    Reqs/sec      7037.81    6142.23   74800.27
    Latency        7.08ms     3.88ms   354.47ms
    HTTP codes:
      1xx - 0, 2xx - 89371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10629
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
    Reqs/sec     24674.24    7802.51   36881.63
    Latency        2.02ms     2.04ms   185.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.60MB/s
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
    Reqs/sec     21569.00    6238.55   31289.77
    Latency        2.32ms     2.03ms   184.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     62087.51    2598.07   65844.91
    Latency      803.25us    64.35us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     19795.04   10915.61   81208.75
    Latency        2.52ms     2.34ms   206.04ms
    HTTP codes:
      1xx - 0, 2xx - 87957, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12043
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12043
    Throughput:     3.94MB/s
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
    Reqs/sec     26113.61    8052.18   61848.56
    Latency        1.91ms     1.89ms   162.38ms
    HTTP codes:
      1xx - 0, 2xx - 97404, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2596
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2596
    Throughput:     5.83MB/s
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
    Reqs/sec     74907.19    2786.72   83311.72
    Latency      664.79us   136.02us     5.53ms
    HTTP codes:
      1xx - 0, 2xx - 96791, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3209
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3209
    Throughput:    11.48MB/s
  ```


