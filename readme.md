## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `100898` | `3497` | `109720` |
| **87%** | [Hyper Express](#hyper-express) | `88131` | `4309` | `90766` |
| **43%** | [Node (Default)](#node-default) | `43517` | `14174` | `90550` |
| **42%** | [Fastify](#fastify) | `42691` | `14201` | `63868` |
| **41%** | [Hono](#hono) | `41850` | `15662` | `58877` |
| **40%** | [Koa](#koa) | `40294` | `15856` | `98212` |
| **11%** | [Carbon](#carbon) | `11358` | `2493` | `16973` |
| **9%** | [Express](#express) | `9243` | `1969` | `13780` |


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
    Reqs/sec     12108.63    8110.02   99309.04
    Latency        4.12ms     3.48ms   298.96ms
    HTTP codes:
      1xx - 0, 2xx - 91483, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8517
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8517
    Throughput:     2.52MB/s
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
    Reqs/sec     10015.86    9526.66   95994.60
    Latency        4.97ms     3.10ms   283.20ms
    HTTP codes:
      1xx - 0, 2xx - 87321, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12679
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12677
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     2.51MB/s
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
    Reqs/sec     43535.31   14681.00   63005.81
    Latency        1.15ms     1.54ms   134.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.88MB/s
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
    Reqs/sec     39097.62   13392.72   58422.82
    Latency        1.28ms     1.58ms   140.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     87320.64    4109.92   90837.02
    Latency      571.44us    59.53us     2.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.40MB/s
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
    Reqs/sec     38000.18   15047.78   91732.71
    Latency        1.31ms     1.75ms   151.93ms
    HTTP codes:
      1xx - 0, 2xx - 92838, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7162
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7162
    Throughput:     8.00MB/s
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
    Reqs/sec     46763.87   15581.07   99456.28
    Latency        1.07ms     1.38ms   114.91ms
    HTTP codes:
      1xx - 0, 2xx - 95246, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4754
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4754
    Throughput:    10.17MB/s
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
    Reqs/sec    100393.51    2609.46  103912.79
    Latency      495.46us   174.41us     8.40ms
    HTTP codes:
      1xx - 0, 2xx - 96070, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3930
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3930
    Throughput:    15.27MB/s
  ```


