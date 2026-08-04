## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68852` | `4977` | `92992` |
| **82%** | [Hyper Express](#hyper-express) | `56527` | `2995` | `61811` |
| **30%** | [Hono](#hono) | `20373` | `6741` | `29852` |
| **29%** | [Node (Default)](#node-default) | `19798` | `4705` | `59380` |
| **28%** | [Fastify](#fastify) | `19556` | `4934` | `35160` |
| **25%** | [Koa](#koa) | `17384` | `7770` | `62302` |
| **11%** | [Carbon](#carbon) | `7283` | `1252` | `10326` |
| **9%** | [Express](#express) | `6016` | `1078` | `8096` |


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
    Reqs/sec      7955.21    6156.08   74333.19
    Latency        6.27ms     4.67ms   396.57ms
    HTTP codes:
      1xx - 0, 2xx - 90306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9694
    Throughput:     1.63MB/s
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
    Reqs/sec      6032.77    1078.68    8103.89
    Latency        8.28ms     3.91ms   373.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     19183.16    4128.51   32190.40
    Latency        2.61ms     2.10ms   190.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.35MB/s
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
    Reqs/sec     20486.50    6478.10   30682.16
    Latency        2.44ms     2.23ms   198.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     57124.36    3223.71   63713.91
    Latency        0.87ms    97.14us     3.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.11MB/s
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
    Reqs/sec     19186.91    9574.66   77144.68
    Latency        2.60ms     2.52ms   221.79ms
    HTTP codes:
      1xx - 0, 2xx - 90361, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9639
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9639
    Throughput:     3.93MB/s
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
    Reqs/sec     19878.61    4371.69   55009.89
    Latency        2.51ms     1.92ms   166.35ms
    HTTP codes:
      1xx - 0, 2xx - 97632, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2368
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2368
    Throughput:     4.44MB/s
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
    Reqs/sec     69466.87    4615.34   83164.02
    Latency      718.20us   212.76us    10.78ms
    HTTP codes:
      1xx - 0, 2xx - 96711, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3289
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3289
    Throughput:    10.62MB/s
  ```


