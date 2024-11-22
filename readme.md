## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73860` | `6792` | `99021` |
| **84%** | [Hyper Express](#hyper-express) | `61859` | `4858` | `82351` |
| **36%** | [Node (Default)](#node-default) | `26609` | `8243` | `55027` |
| **32%** | [Fastify](#fastify) | `23723` | `7010` | `34730` |
| **27%** | [Hono](#hono) | `20095` | `5545` | `28670` |
| **25%** | [Koa](#koa) | `18255` | `6223` | `46567` |
| **11%** | [Carbon](#carbon) | `8411` | `1506` | `10442` |
| **9%** | [Express](#express) | `6569` | `1068` | `8595` |


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
    Reqs/sec      8610.20    4199.13   56684.13
    Latency        5.78ms     4.22ms   366.25ms
    HTTP codes:
      1xx - 0, 2xx - 94051, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5949
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5949
    Throughput:     1.84MB/s
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
    Reqs/sec      6678.13    1105.31    8529.89
    Latency        7.48ms     3.53ms   337.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     23309.29    6539.07   34955.98
    Latency        2.14ms     2.02ms   182.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20587.74    5715.08   28507.45
    Latency        2.43ms     2.03ms   182.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     61446.29    3843.21   64431.95
    Latency      809.46us    65.32us     3.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     17801.38    5969.66   47368.74
    Latency        2.81ms     2.36ms   213.63ms
    HTTP codes:
      1xx - 0, 2xx - 95549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4451
    Throughput:     3.84MB/s
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
    Reqs/sec     25367.30    7646.29   52410.18
    Latency        1.96ms     1.87ms   159.50ms
    HTTP codes:
      1xx - 0, 2xx - 97434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2566
    Throughput:     5.68MB/s
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
    Reqs/sec     74328.13    6483.77   84422.03
    Latency      670.92us   222.45us    10.20ms
    HTTP codes:
      1xx - 0, 2xx - 97810, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2190
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2190
    Throughput:    11.50MB/s
  ```


