## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73499` | `5516` | `78813` |
| **86%** | [Hyper Express](#hyper-express) | `62986` | `3047` | `65892` |
| **35%** | [Node (Default)](#node-default) | `25382` | `6661` | `42432` |
| **33%** | [Fastify](#fastify) | `24513` | `7399` | `37136` |
| **30%** | [Hono](#hono) | `21698` | `6438` | `30783` |
| **25%** | [Koa](#koa) | `18266` | `5969` | `56201` |
| **11%** | [Carbon](#carbon) | `8247` | `1451` | `10736` |
| **9%** | [Express](#express) | `6602` | `1025` | `8491` |


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
    Reqs/sec      8704.82    3632.19   50238.28
    Latency        5.73ms     4.15ms   364.45ms
    HTTP codes:
      1xx - 0, 2xx - 95263, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4737
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4737
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
    Reqs/sec      6594.81    1031.47    8465.96
    Latency        7.58ms     3.54ms   336.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24649.21    7180.28   37682.91
    Latency        2.03ms     1.85ms   169.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     21110.42    6072.31   30967.66
    Latency        2.37ms     1.94ms   175.62ms
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
    Reqs/sec     62615.18    4106.63   79629.61
    Latency      799.36us    71.76us     4.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     18942.80    6661.97   56502.20
    Latency        2.64ms     2.23ms   196.66ms
    HTTP codes:
      1xx - 0, 2xx - 94250, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5750
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5750
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
    Reqs/sec     25243.67    6823.14   63335.22
    Latency        1.98ms     1.77ms   152.45ms
    HTTP codes:
      1xx - 0, 2xx - 97203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2797
    Throughput:     5.62MB/s
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
    Reqs/sec     74085.33    6041.02   81588.88
    Latency      670.85us   211.37us    14.71ms
    HTTP codes:
      1xx - 0, 2xx - 97716, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2284
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2284
    Throughput:    11.45MB/s
  ```


