## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `66889` | `5740` | `78034` |
| **87%** | [Hyper Express](#hyper-express) | `58280` | `3875` | `65347` |
| **31%** | [Fastify](#fastify) | `20427` | `5026` | `37040` |
| **30%** | [Hono](#hono) | `20242` | `6069` | `30247` |
| **30%** | [Node (Default)](#node-default) | `20034` | `5248` | `62869` |
| **27%** | [Koa](#koa) | `18059` | `7802` | `62607` |
| **11%** | [Carbon](#carbon) | `7493` | `1374` | `10339` |
| **9%** | [Express](#express) | `6171` | `1083` | `8336` |


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
    Reqs/sec      7798.22    4838.25   58598.77
    Latency        6.40ms     4.75ms   401.12ms
    HTTP codes:
      1xx - 0, 2xx - 92963, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7037
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7037
    Throughput:     1.65MB/s
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
    Reqs/sec      6214.00    1111.48    8203.43
    Latency        8.04ms     3.80ms   366.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     19398.30    4404.59   35030.20
    Latency        2.58ms     2.17ms   195.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.40MB/s
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
    Reqs/sec     21058.07    6328.69   30265.62
    Latency        2.37ms     2.24ms   197.06ms
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
    Reqs/sec     58187.11    3402.90   64287.00
    Latency        0.86ms    97.98us     3.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.27MB/s
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
    Reqs/sec     19198.44    8888.65   77124.11
    Latency        2.59ms     2.43ms   213.38ms
    HTTP codes:
      1xx - 0, 2xx - 91904, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8096
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8096
    Throughput:     3.99MB/s
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
    Reqs/sec     19835.05    5543.67   74679.49
    Latency        2.52ms     1.97ms   166.95ms
    HTTP codes:
      1xx - 0, 2xx - 96687, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3313
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3313
    Throughput:     4.39MB/s
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
    Reqs/sec     70034.60    4416.49   84882.54
    Latency      712.29us   155.92us     6.87ms
    HTTP codes:
      1xx - 0, 2xx - 97254, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2746
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2746
    Throughput:    10.76MB/s
  ```


