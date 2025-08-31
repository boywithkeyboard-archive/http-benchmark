## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75303` | `2808` | `85039` |
| **84%** | [Hyper Express](#hyper-express) | `63058` | `3448` | `65649` |
| **33%** | [Node (Default)](#node-default) | `24828` | `8032` | `57598` |
| **31%** | [Fastify](#fastify) | `23587` | `6963` | `35895` |
| **26%** | [Hono](#hono) | `19659` | `5271` | `29365` |
| **24%** | [Koa](#koa) | `18420` | `8795` | `75095` |
| **11%** | [Carbon](#carbon) | `7991` | `1342` | `10228` |
| **9%** | [Express](#express) | `6413` | `1039` | `8428` |


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
    Reqs/sec      8997.83    6914.85   76672.46
    Latency        5.53ms     4.29ms   370.59ms
    HTTP codes:
      1xx - 0, 2xx - 89041, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10959
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10959
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
    Reqs/sec      6405.69    1086.54    8398.35
    Latency        7.80ms     3.75ms   358.87ms
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
    Reqs/sec     22835.11    6505.05   35946.18
    Latency        2.19ms     2.12ms   188.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.18MB/s
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
    Reqs/sec     19338.84    5153.72   29004.48
    Latency        2.58ms     2.00ms   178.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.37MB/s
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
    Reqs/sec     62621.26    3708.41   65930.39
    Latency      796.14us    73.62us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18445.14    7751.25   65772.50
    Latency        2.71ms     2.41ms   213.66ms
    HTTP codes:
      1xx - 0, 2xx - 93045, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6955
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6955
    Throughput:     3.88MB/s
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
    Reqs/sec     24264.01    8525.80   77063.35
    Latency        2.05ms     1.94ms   164.86ms
    HTTP codes:
      1xx - 0, 2xx - 95808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4192
    Throughput:     5.33MB/s
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
    Reqs/sec     74631.59    3202.15   81315.99
    Latency      667.48us   144.24us     6.02ms
    HTTP codes:
      1xx - 0, 2xx - 96896, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3104
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3104
    Throughput:    11.45MB/s
  ```


