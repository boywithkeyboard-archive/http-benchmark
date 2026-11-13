## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `136226` | `5224` | `144480` |
| **87%** | [Hyper Express](#hyper-express) | `118466` | `7170` | `125756` |
| **31%** | [Node (Default)](#node-default) | `42505` | `12072` | `122676` |
| **29%** | [Fastify](#fastify) | `39595` | `6365` | `49085` |
| **28%** | [Koa](#koa) | `38215` | `17144` | `132287` |
| **27%** | [Hono](#hono) | `37193` | `6558` | `46626` |
| **12%** | [Carbon](#carbon) | `16867` | `4683` | `25916` |
| **9%** | [Express](#express) | `11780` | `2253` | `16718` |


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
    Reqs/sec     19650.96   16263.75  125120.39
    Latency        2.53ms     3.37ms   282.24ms
    HTTP codes:
      1xx - 0, 2xx - 85195, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14805
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14805
    Throughput:     3.82MB/s
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
    Reqs/sec     12893.83   12806.40  130791.84
    Latency        3.85ms     3.02ms   270.44ms
    HTTP codes:
      1xx - 0, 2xx - 85825, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14175
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14175
    Throughput:     3.19MB/s
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
    Reqs/sec     48531.63   24698.36  129217.14
    Latency        1.03ms     1.23ms    94.61ms
    HTTP codes:
      1xx - 0, 2xx - 74778, 3xx - 0, 4xx - 0, 5xx - 0
      others - 25222
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 25221
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     8.25MB/s
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
    Reqs/sec     39355.90   16550.68  132543.52
    Latency        1.27ms     1.33ms   107.75ms
    HTTP codes:
      1xx - 0, 2xx - 89026, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10974
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10974
    Throughput:     7.91MB/s
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
    Reqs/sec    116336.55    6887.43  122639.49
    Latency      427.12us   245.28us    10.08ms
    HTTP codes:
      1xx - 0, 2xx - 90619, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9381
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9381
    Throughput:    14.98MB/s
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
    Reqs/sec     37312.52   15480.89  131461.40
    Latency        1.33ms     1.32ms   106.00ms
    HTTP codes:
      1xx - 0, 2xx - 88724, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11276
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11276
    Throughput:     7.54MB/s
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
    Reqs/sec     43414.24   13872.99  138064.47
    Latency        1.15ms     1.05ms    82.24ms
    HTTP codes:
      1xx - 0, 2xx - 93399, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6601
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6601
    Throughput:     9.30MB/s
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
    Reqs/sec    137059.27    5251.30  146752.81
    Latency      361.16us   106.32us     5.26ms
    HTTP codes:
      1xx - 0, 2xx - 94157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5843
    Throughput:    20.42MB/s
  ```


