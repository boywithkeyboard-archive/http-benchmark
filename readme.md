## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71773` | `5109` | `85654` |
| **86%** | [Hyper Express](#hyper-express) | `61689` | `2891` | `64928` |
| **36%** | [Node (Default)](#node-default) | `25585` | `7328` | `53138` |
| **34%** | [Fastify](#fastify) | `24159` | `7310` | `35946` |
| **29%** | [Hono](#hono) | `20578` | `5834` | `30231` |
| **25%** | [Koa](#koa) | `18215` | `6275` | `58006` |
| **12%** | [Carbon](#carbon) | `8324` | `1445` | `10454` |
| **9%** | [Express](#express) | `6470` | `1035` | `8507` |


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
    Reqs/sec      8482.73    3130.58   49635.18
    Latency        5.88ms     4.17ms   369.48ms
    HTTP codes:
      1xx - 0, 2xx - 96084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3916
    Throughput:     1.85MB/s
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
    Reqs/sec      6536.39    1031.15    8561.99
    Latency        7.65ms     3.64ms   348.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24293.19    7222.00   36135.98
    Latency        2.06ms     1.98ms   181.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21061.94    5555.45   28994.23
    Latency        2.37ms     2.02ms   181.00ms
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
    Reqs/sec     61701.18    3947.21   65411.39
    Latency      808.16us    76.23us     4.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     18930.39    6692.59   54531.08
    Latency        2.64ms     2.38ms   207.57ms
    HTTP codes:
      1xx - 0, 2xx - 94460, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5540
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5540
    Throughput:     4.04MB/s
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
    Reqs/sec     26338.51    8001.26   51150.13
    Latency        1.89ms     2.01ms   173.67ms
    HTTP codes:
      1xx - 0, 2xx - 97477, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2523
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2523
    Throughput:     5.89MB/s
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
    Reqs/sec     72611.63    5444.52   79820.96
    Latency      685.47us   175.47us    11.68ms
    HTTP codes:
      1xx - 0, 2xx - 97610, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2390
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2390
    Throughput:    11.22MB/s
  ```


