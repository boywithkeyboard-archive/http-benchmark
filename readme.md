## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74159` | `6387` | `91179` |
| **84%** | [Hyper Express](#hyper-express) | `62586` | `2278` | `72712` |
| **36%** | [Node (Default)](#node-default) | `26723` | `8329` | `51669` |
| **31%** | [Fastify](#fastify) | `22778` | `6671` | `34738` |
| **28%** | [Hono](#hono) | `20916` | `6388` | `29651` |
| **24%** | [Koa](#koa) | `17821` | `5895` | `49320` |
| **11%** | [Carbon](#carbon) | `8133` | `1400` | `10245` |
| **9%** | [Express](#express) | `6369` | `1019` | `8471` |


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
    Reqs/sec      8678.13    3817.39   50407.79
    Latency        5.75ms     4.23ms   363.40ms
    HTTP codes:
      1xx - 0, 2xx - 94836, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5164
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5164
    Throughput:     1.87MB/s
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
    Reqs/sec      6363.30     984.75    8328.66
    Latency        7.85ms     3.64ms   349.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23855.85    7509.93   34824.59
    Latency        2.09ms     2.08ms   186.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     20720.11    6009.31   29233.11
    Latency        2.41ms     2.01ms   180.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62642.55    3329.44   79514.20
    Latency      798.13us    72.48us     3.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     17591.43    5973.83   53069.27
    Latency        2.83ms     2.33ms   207.75ms
    HTTP codes:
      1xx - 0, 2xx - 94318, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5682
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5682
    Throughput:     3.75MB/s
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
    Reqs/sec     25296.25    7488.46   55023.62
    Latency        1.97ms     1.99ms   166.64ms
    HTTP codes:
      1xx - 0, 2xx - 96864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3131
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 5
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
    Reqs/sec     73051.54    5572.94   84877.90
    Latency      682.42us   194.45us     8.90ms
    HTTP codes:
      1xx - 0, 2xx - 97643, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2357
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2357
    Throughput:    11.29MB/s
  ```


