## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `123139` | `2715` | `129340` |
| **91%** | [Hyper Express](#hyper-express) | `111970` | `5191` | `117377` |
| **57%** | [Node (Default)](#node-default) | `70567` | `15762` | `116845` |
| **54%** | [Fastify](#fastify) | `66949` | `16886` | `88816` |
| **53%** | [Hono](#hono) | `65100` | `18964` | `83741` |
| **52%** | [Koa](#koa) | `63624` | `18931` | `117045` |
| **21%** | [Carbon](#carbon) | `26432` | `7938` | `40130` |
| **15%** | [Express](#express) | `18126` | `3736` | `25718` |


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
    Reqs/sec     27895.60   15927.04  121084.67
    Latency        1.79ms     3.03ms   251.05ms
    HTTP codes:
      1xx - 0, 2xx - 89299, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10701
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10701
    Throughput:     5.65MB/s
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
    Reqs/sec     19387.17   11383.69  117479.82
    Latency        2.57ms     2.16ms   189.92ms
    HTTP codes:
      1xx - 0, 2xx - 92008, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7992
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7992
    Throughput:     5.12MB/s
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
    Reqs/sec     76088.14   23111.44  130208.12
    Latency      654.64us     1.01ms    78.84ms
    HTTP codes:
      1xx - 0, 2xx - 79564, 3xx - 0, 4xx - 0, 5xx - 0
      others - 20436
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 20436
    Throughput:    13.75MB/s
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
    Reqs/sec     65164.83   20884.04  137700.90
    Latency      765.61us     1.03ms    77.09ms
    HTTP codes:
      1xx - 0, 2xx - 91425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8575
    Throughput:    13.46MB/s
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
    Reqs/sec    113802.77    6425.13  120210.83
    Latency      437.68us   215.78us     9.39ms
    HTTP codes:
      1xx - 0, 2xx - 91918, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8082
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8082
    Throughput:    14.86MB/s
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
    Reqs/sec     60632.01   19396.90  117145.87
    Latency      820.21us     1.14ms    96.37ms
    HTTP codes:
      1xx - 0, 2xx - 92745, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7255
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7255
    Throughput:    12.75MB/s
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
    Reqs/sec     71533.68   17534.92  115249.09
    Latency      695.74us     0.94ms    65.06ms
    HTTP codes:
      1xx - 0, 2xx - 96243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3757
    Throughput:    15.76MB/s
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
    Reqs/sec    123643.11    4280.84  129334.46
    Latency      402.57us   120.00us     5.89ms
    HTTP codes:
      1xx - 0, 2xx - 95845, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4155
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4155
    Throughput:    18.75MB/s
  ```


