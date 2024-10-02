## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72537` | `5142` | `81779` |
| **86%** | [Hyper Express](#hyper-express) | `62201` | `4010` | `66796` |
| **36%** | [Node (Default)](#node-default) | `26155` | `7901` | `54973` |
| **33%** | [Fastify](#fastify) | `23844` | `7437` | `35289` |
| **29%** | [Hono](#hono) | `20980` | `6060` | `30186` |
| **25%** | [Koa](#koa) | `18187` | `6732` | `58395` |
| **11%** | [Carbon](#carbon) | `8108` | `1476` | `10378` |
| **9%** | [Express](#express) | `6167` | `942` | `8219` |


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
    Reqs/sec      8333.26    5195.89   59488.55
    Latency        5.99ms     4.53ms   391.25ms
    HTTP codes:
      1xx - 0, 2xx - 91730, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8270
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8270
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
    Reqs/sec      6179.17     977.34    8199.93
    Latency        8.09ms     3.85ms   366.96ms
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
    Reqs/sec     24696.63    7959.26   37367.03
    Latency        2.02ms     2.14ms   189.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.60MB/s
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
    Reqs/sec     21156.80    5514.12   29842.59
    Latency        2.36ms     2.02ms   182.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     63010.41    4008.48   69459.85
    Latency      791.10us    73.51us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18477.38    7234.27   55794.97
    Latency        2.70ms     2.42ms   209.12ms
    HTTP codes:
      1xx - 0, 2xx - 92705, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7295
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7295
    Throughput:     3.88MB/s
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
    Reqs/sec     26290.65    8540.94   55248.58
    Latency        1.89ms     1.92ms   164.67ms
    HTTP codes:
      1xx - 0, 2xx - 96483, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3517
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3517
    Throughput:     5.81MB/s
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
    Reqs/sec     73083.11    4846.88   82946.57
    Latency      680.96us   196.55us    10.66ms
    HTTP codes:
      1xx - 0, 2xx - 96635, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3365
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3365
    Throughput:    11.17MB/s
  ```


