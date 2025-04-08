## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73752` | `5764` | `86721` |
| **84%** | [Hyper Express](#hyper-express) | `61790` | `3670` | `66633` |
| **35%** | [Node (Default)](#node-default) | `26015` | `7238` | `49398` |
| **34%** | [Fastify](#fastify) | `24779` | `7564` | `35243` |
| **28%** | [Hono](#hono) | `20419` | `5561` | `29983` |
| **25%** | [Koa](#koa) | `18414` | `6624` | `55369` |
| **11%** | [Carbon](#carbon) | `8225` | `1378` | `10326` |
| **8%** | [Express](#express) | `6262` | `962` | `8403` |


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
    Reqs/sec      8503.33    4473.11   53267.41
    Latency        5.87ms     4.48ms   381.36ms
    HTTP codes:
      1xx - 0, 2xx - 93235, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6765
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6765
    Throughput:     1.80MB/s
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
    Reqs/sec      6317.48    1014.19    8338.41
    Latency        7.91ms     3.78ms   360.59ms
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
    Reqs/sec     24876.91    7695.83   37158.43
    Latency        2.01ms     2.10ms   187.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.64MB/s
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
    Reqs/sec     21058.90    6350.98   29928.42
    Latency        2.37ms     2.05ms   184.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     62186.19    3661.85   68759.09
    Latency      801.86us    67.78us     3.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     18494.14    6575.57   59320.15
    Latency        2.70ms     2.36ms   208.00ms
    HTTP codes:
      1xx - 0, 2xx - 94154, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5846
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5846
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
    Reqs/sec     26395.76    8104.19   52185.33
    Latency        1.89ms     1.80ms   157.75ms
    HTTP codes:
      1xx - 0, 2xx - 97942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2058
    Throughput:     5.92MB/s
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
    Reqs/sec     73352.29    5636.98   84506.91
    Latency      678.56us   205.41us     8.95ms
    HTTP codes:
      1xx - 0, 2xx - 96864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3136
    Throughput:    11.25MB/s
  ```


