## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75904` | `5022` | `97775` |
| **81%** | [Hyper Express](#hyper-express) | `61705` | `3170` | `65220` |
| **31%** | [Fastify](#fastify) | `23489` | `7161` | `34262` |
| **31%** | [Node (Default)](#node-default) | `23374` | `7895` | `66118` |
| **24%** | [Hono](#hono) | `18349` | `4924` | `28293` |
| **23%** | [Koa](#koa) | `17274` | `8903` | `79317` |
| **10%** | [Carbon](#carbon) | `7818` | `1446` | `10329` |
| **8%** | [Express](#express) | `6007` | `1059` | `8160` |


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
    Reqs/sec      8274.02    5203.00   65598.75
    Latency        6.03ms     4.58ms   389.53ms
    HTTP codes:
      1xx - 0, 2xx - 92349, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7651
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7651
    Throughput:     1.74MB/s
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
    Reqs/sec      6198.17    1040.94    8229.89
    Latency        8.06ms     3.73ms   360.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     23103.38    7334.97   34838.84
    Latency        2.16ms     2.04ms   183.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     18618.27    5131.56   28628.71
    Latency        2.68ms     2.14ms   193.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.21MB/s
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
    Reqs/sec     62909.00    3569.14   67051.69
    Latency      792.78us    88.76us     2.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     17437.66    7692.81   66141.74
    Latency        2.86ms     2.37ms   209.76ms
    HTTP codes:
      1xx - 0, 2xx - 92867, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7133
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7133
    Throughput:     3.67MB/s
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
    Reqs/sec     25636.97    8681.01   70358.80
    Latency        1.95ms     1.91ms   167.75ms
    HTTP codes:
      1xx - 0, 2xx - 96271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3729
    Throughput:     5.65MB/s
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
    Reqs/sec     75744.28    3914.47   89250.39
    Latency      657.56us   147.35us     6.13ms
    HTTP codes:
      1xx - 0, 2xx - 97371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2629
    Throughput:    11.67MB/s
  ```


