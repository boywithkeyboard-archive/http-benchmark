## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73941` | `6100` | `87085` |
| **84%** | [Hyper Express](#hyper-express) | `62293` | `3311` | `69044` |
| **37%** | [Node (Default)](#node-default) | `27039` | `8175` | `56182` |
| **34%** | [Fastify](#fastify) | `25386` | `7956` | `36136` |
| **28%** | [Hono](#hono) | `20726` | `5609` | `29303` |
| **24%** | [Koa](#koa) | `18092` | `7094` | `52734` |
| **11%** | [Carbon](#carbon) | `8346` | `1446` | `10534` |
| **9%** | [Express](#express) | `6529` | `1072` | `8643` |


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
    Reqs/sec      8728.41    4115.37   49655.66
    Latency        5.68ms     4.13ms   369.32ms
    HTTP codes:
      1xx - 0, 2xx - 93698, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6302
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6302
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
    Reqs/sec      6663.33    1089.09    8647.80
    Latency        7.50ms     3.54ms   338.21ms
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
    Reqs/sec     24051.63    7078.06   35763.66
    Latency        2.08ms     1.98ms   178.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20678.83    5297.83   29377.58
    Latency        2.42ms     1.97ms   179.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     62592.17    4453.08   77495.55
    Latency      799.06us    91.48us     4.01ms
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
    Reqs/sec     18536.91    6640.73   60037.95
    Latency        2.69ms     2.33ms   204.31ms
    HTTP codes:
      1xx - 0, 2xx - 94467, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5533
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5533
    Throughput:     3.96MB/s
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
    Reqs/sec     26282.03    7674.91   56779.04
    Latency        1.90ms     1.94ms   180.68ms
    HTTP codes:
      1xx - 0, 2xx - 97543, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2457
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2457
    Throughput:     5.87MB/s
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
    Reqs/sec     73941.44    5335.07   86009.70
    Latency      673.06us   206.40us    12.29ms
    HTTP codes:
      1xx - 0, 2xx - 96447, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3553
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3553
    Throughput:    11.28MB/s
  ```


