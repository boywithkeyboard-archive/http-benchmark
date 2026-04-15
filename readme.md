## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `125361` | `10061` | `143880` |
| **80%** | [Hyper Express](#hyper-express) | `100703` | `7460` | `106821` |
| **29%** | [Node (Default)](#node-default) | `36450` | `13144` | `109572` |
| **29%** | [Fastify](#fastify) | `35957` | `8737` | `53120` |
| **25%** | [Hono](#hono) | `31351` | `7262` | `47847` |
| **24%** | [Koa](#koa) | `30568` | `14256` | `105969` |
| **9%** | [Carbon](#carbon) | `11623` | `2475` | `17674` |
| **7%** | [Express](#express) | `8547` | `1541` | `11933` |


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
    Reqs/sec     13220.47    9750.22   96104.46
    Latency        3.78ms     3.64ms   310.08ms
    HTTP codes:
      1xx - 0, 2xx - 89503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10497
    Throughput:     2.69MB/s
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
    Reqs/sec      9777.13   10758.07  104307.81
    Latency        5.09ms     3.57ms   320.34ms
    HTTP codes:
      1xx - 0, 2xx - 84832, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15168
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15168
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
    Reqs/sec     33677.74    7085.65   45162.43
    Latency        1.48ms     1.48ms   131.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.64MB/s
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
    Reqs/sec     31150.65    6780.10   42619.09
    Latency        1.60ms     1.61ms   143.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.03MB/s
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
    Reqs/sec     98810.52   10575.53  111951.37
    Latency      504.74us    97.19us     3.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.02MB/s
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
    Reqs/sec     32907.60   16934.73  123797.20
    Latency        1.51ms     1.77ms   149.09ms
    HTTP codes:
      1xx - 0, 2xx - 85778, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14222
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14222
    Throughput:     6.39MB/s
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
    Reqs/sec     34941.16    9399.16   95450.77
    Latency        1.43ms     1.34ms   113.65ms
    HTTP codes:
      1xx - 0, 2xx - 96084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3916
    Throughput:     7.69MB/s
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
    Reqs/sec    119881.03   12596.73  143436.44
    Latency      413.22us   187.84us     7.57ms
    HTTP codes:
      1xx - 0, 2xx - 96597, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3403
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3403
    Throughput:    18.32MB/s
  ```


