## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `121314` | `8334` | `139500` |
| **83%** | [Hyper Express](#hyper-express) | `100324` | `6546` | `109171` |
| **31%** | [Node (Default)](#node-default) | `37678` | `9555` | `97376` |
| **28%** | [Fastify](#fastify) | `33973` | `8408` | `55083` |
| **26%** | [Koa](#koa) | `31724` | `15751` | `115497` |
| **26%** | [Hono](#hono) | `31260` | `6811` | `46986` |
| **10%** | [Carbon](#carbon) | `11970` | `2445` | `16698` |
| **7%** | [Express](#express) | `8848` | `1800` | `11648` |


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
    Reqs/sec     13422.81   11530.64  117165.79
    Latency        3.72ms     3.71ms   321.76ms
    HTTP codes:
      1xx - 0, 2xx - 87255, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12745
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12745
    Throughput:     2.66MB/s
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
    Reqs/sec      9760.83   10861.98  109066.30
    Latency        5.11ms     3.39ms   308.76ms
    HTTP codes:
      1xx - 0, 2xx - 85784, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14216
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14216
    Throughput:     2.40MB/s
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
    Reqs/sec     34594.03    8099.60   54906.20
    Latency        1.44ms     1.51ms   133.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.85MB/s
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
    Reqs/sec     31660.05    7406.30   49296.69
    Latency        1.58ms     1.53ms   135.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.15MB/s
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
    Reqs/sec    101649.90    6286.40  109357.49
    Latency      490.19us    47.92us     1.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.44MB/s
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
    Reqs/sec     31989.54   13751.26  100119.92
    Latency        1.56ms     1.78ms   154.50ms
    HTTP codes:
      1xx - 0, 2xx - 90544, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9456
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9456
    Throughput:     6.56MB/s
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
    Reqs/sec     38484.51   15488.52  121505.12
    Latency        1.30ms     1.37ms   118.81ms
    HTTP codes:
      1xx - 0, 2xx - 92030, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7970
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7970
    Throughput:     8.10MB/s
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
    Reqs/sec    121595.12    9422.84  142323.61
    Latency      408.88us   155.32us     6.45ms
    HTTP codes:
      1xx - 0, 2xx - 96313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3687
    Throughput:    18.52MB/s
  ```


