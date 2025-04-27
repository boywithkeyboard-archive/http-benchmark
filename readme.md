## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74801` | `5670` | `87516` |
| **82%** | [Hyper Express](#hyper-express) | `61605` | `3824` | `66616` |
| **34%** | [Node (Default)](#node-default) | `25745` | `7729` | `48867` |
| **33%** | [Fastify](#fastify) | `24642` | `7817` | `37409` |
| **27%** | [Hono](#hono) | `20218` | `5422` | `29769` |
| **26%** | [Koa](#koa) | `19661` | `7320` | `57894` |
| **11%** | [Carbon](#carbon) | `8147` | `1360` | `10110` |
| **9%** | [Express](#express) | `6451` | `1056` | `8407` |


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
    Reqs/sec      8856.74    4682.19   57977.88
    Latency        5.64ms     4.24ms   364.31ms
    HTTP codes:
      1xx - 0, 2xx - 93234, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6766
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6766
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
    Reqs/sec      7148.83    5290.99   63618.45
    Latency        6.98ms     4.01ms   368.08ms
    HTTP codes:
      1xx - 0, 2xx - 90054, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9946
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9946
    Throughput:     1.84MB/s
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
    Reqs/sec     23184.85    6531.99   36368.95
    Latency        2.16ms     1.93ms   176.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     21065.71    6029.66   29635.93
    Latency        2.37ms     2.06ms   186.55ms
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
    Reqs/sec     61948.11    4003.12   67167.98
    Latency      804.77us   111.82us     9.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18634.51    6732.41   54133.03
    Latency        2.67ms     2.30ms   199.72ms
    HTTP codes:
      1xx - 0, 2xx - 94157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5843
    Throughput:     3.97MB/s
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
    Reqs/sec     26676.79    8138.50   54977.90
    Latency        1.87ms     1.90ms   163.64ms
    HTTP codes:
      1xx - 0, 2xx - 97818, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2182
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2182
    Throughput:     5.98MB/s
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
    Reqs/sec     74996.68    5473.69   91133.49
    Latency      664.46us   148.03us     5.96ms
    HTTP codes:
      1xx - 0, 2xx - 97937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2063
    Throughput:    11.61MB/s
  ```


