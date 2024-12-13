## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74014` | `5659` | `80030` |
| **85%** | [Hyper Express](#hyper-express) | `63118` | `3031` | `70567` |
| **36%** | [Node (Default)](#node-default) | `26888` | `8709` | `53700` |
| **32%** | [Fastify](#fastify) | `23524` | `6496` | `35498` |
| **28%** | [Hono](#hono) | `20627` | `5634` | `30210` |
| **23%** | [Koa](#koa) | `17284` | `6086` | `55032` |
| **11%** | [Carbon](#carbon) | `8212` | `1387` | `10474` |
| **9%** | [Express](#express) | `6603` | `1055` | `8560` |


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
    Reqs/sec      8717.82    4278.76   53150.49
    Latency        5.73ms     4.13ms   357.63ms
    HTTP codes:
      1xx - 0, 2xx - 93735, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6265
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6265
    Throughput:     1.86MB/s
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
    Reqs/sec      6395.13     953.83    8512.24
    Latency        7.82ms     3.60ms   344.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24136.35    7431.52   35235.26
    Latency        2.07ms     2.00ms   183.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     19916.16    5258.21   28191.22
    Latency        2.51ms     2.00ms   183.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.50MB/s
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
    Reqs/sec     63407.37    4057.91   72946.51
    Latency      786.10us    77.07us     3.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18141.86    5290.69   43893.37
    Latency        2.75ms     2.43ms   209.63ms
    HTTP codes:
      1xx - 0, 2xx - 96201, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3799
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3799
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
    Reqs/sec     25653.51    7445.90   58067.36
    Latency        1.95ms     1.92ms   165.41ms
    HTTP codes:
      1xx - 0, 2xx - 97397, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2603
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2603
    Throughput:     5.71MB/s
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
    Reqs/sec     74110.97    6426.16   91914.61
    Latency      672.73us   257.44us    23.95ms
    HTTP codes:
      1xx - 0, 2xx - 98131, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1869
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1869
    Throughput:    11.51MB/s
  ```


