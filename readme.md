## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `116806` | `6980` | `130942` |
| **81%** | [Hyper Express](#hyper-express) | `94779` | `6077` | `99378` |
| **30%** | [Node (Default)](#node-default) | `35594` | `10480` | `95976` |
| **30%** | [Fastify](#fastify) | `34634` | `8427` | `53231` |
| **26%** | [Koa](#koa) | `30703` | `14645` | `105579` |
| **25%** | [Hono](#hono) | `29653` | `6171` | `46477` |
| **10%** | [Carbon](#carbon) | `11554` | `2273` | `15810` |
| **7%** | [Express](#express) | `8292` | `1643` | `11318` |


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
    Reqs/sec     12715.79   11364.42  112328.39
    Latency        3.92ms     3.70ms   316.72ms
    HTTP codes:
      1xx - 0, 2xx - 86760, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13240
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13240
    Throughput:     2.51MB/s
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
    Reqs/sec      9644.43   10250.32  106192.03
    Latency        5.17ms     3.47ms   314.53ms
    HTTP codes:
      1xx - 0, 2xx - 86231, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13769
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13769
    Throughput:     2.38MB/s
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
    Reqs/sec     39669.01   17321.56   93407.19
    Latency        1.25ms     1.66ms   139.62ms
    HTTP codes:
      1xx - 0, 2xx - 80009, 3xx - 0, 4xx - 0, 5xx - 0
      others - 19991
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 19953
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 38
    Throughput:     7.21MB/s
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
    Reqs/sec     31823.24   13171.20  106002.40
    Latency        1.57ms     1.68ms   142.40ms
    HTTP codes:
      1xx - 0, 2xx - 90011, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9989
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9989
    Throughput:     6.48MB/s
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
    Reqs/sec     95998.54    6203.14  104571.09
    Latency      517.05us   252.81us    11.54ms
    HTTP codes:
      1xx - 0, 2xx - 91464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8536
    Throughput:    12.49MB/s
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
    Reqs/sec     31529.90   13773.36   99889.87
    Latency        1.58ms     1.78ms   151.34ms
    HTTP codes:
      1xx - 0, 2xx - 88864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11136
    Throughput:     6.33MB/s
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
    Reqs/sec     36133.39   10037.16   95961.13
    Latency        1.38ms     1.38ms   120.70ms
    HTTP codes:
      1xx - 0, 2xx - 96047, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3953
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3953
    Throughput:     7.94MB/s
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
    Reqs/sec    116722.87    5957.59  128562.03
    Latency      425.97us   147.92us     6.27ms
    HTTP codes:
      1xx - 0, 2xx - 92546, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7454
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7454
    Throughput:    17.10MB/s
  ```


