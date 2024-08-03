## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73497` | `5711` | `83785` |
| **87%** | [Hyper Express](#hyper-express) | `63866` | `4476` | `70545` |
| **34%** | [Node (Default)](#node-default) | `24997` | `7638` | `63524` |
| **33%** | [Fastify](#fastify) | `23907` | `7585` | `35714` |
| **27%** | [Hono](#hono) | `20100` | `5540` | `29930` |
| **25%** | [Koa](#koa) | `18161` | `6229` | `49472` |
| **11%** | [Carbon](#carbon) | `8121` | `1368` | `10412` |
| **9%** | [Express](#express) | `6417` | `1009` | `8471` |


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
    Reqs/sec      8662.61    4716.44   65570.69
    Latency        5.76ms     4.20ms   364.17ms
    HTTP codes:
      1xx - 0, 2xx - 92785, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7215
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7215
    Throughput:     1.82MB/s
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
    Reqs/sec      6490.93    1050.67    8443.36
    Latency        7.70ms     3.57ms   346.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23951.10    7842.62   36565.61
    Latency        2.09ms     2.12ms   189.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20302.44    5485.35   30086.78
    Latency        2.46ms     1.99ms   183.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     62773.74    3198.72   69816.76
    Latency      793.69us    71.91us     3.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     18318.95    6000.78   49027.75
    Latency        2.72ms     2.33ms   204.03ms
    HTTP codes:
      1xx - 0, 2xx - 95315, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4685
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4685
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
    Reqs/sec     25608.22    7839.65   55147.02
    Latency        1.95ms     1.96ms   161.70ms
    HTTP codes:
      1xx - 0, 2xx - 96848, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3152
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3152
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
    Reqs/sec     74349.06    6787.59   94393.11
    Latency      670.08us   234.94us    10.78ms
    HTTP codes:
      1xx - 0, 2xx - 97150, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2850
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2850
    Throughput:    11.43MB/s
  ```


