## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72715` | `4724` | `85199` |
| **84%** | [Hyper Express](#hyper-express) | `61069` | `3955` | `68253` |
| **31%** | [Hono](#hono) | `22849` | `6422` | `30580` |
| **31%** | [Node (Default)](#node-default) | `22448` | `6193` | `65399` |
| **30%** | [Fastify](#fastify) | `21963` | `6738` | `37010` |
| **26%** | [Koa](#koa) | `18585` | `8974` | `75008` |
| **10%** | [Carbon](#carbon) | `7612` | `1236` | `10516` |
| **9%** | [Express](#express) | `6356` | `1111` | `8377` |


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
    Reqs/sec      8040.67    5279.08   63037.43
    Latency        6.21ms     4.32ms   372.25ms
    HTTP codes:
      1xx - 0, 2xx - 92114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7886
    Throughput:     1.68MB/s
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
    Reqs/sec      6246.22    1043.12    8389.36
    Latency        8.00ms     3.87ms   365.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     23301.76    6883.64   35643.46
    Latency        2.14ms     2.06ms   185.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     22867.16    6583.44   31232.62
    Latency        2.19ms     2.01ms   184.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.16MB/s
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
    Reqs/sec     60975.81    3566.42   66959.90
    Latency      818.03us    92.58us     3.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     19401.29    8571.67   70924.61
    Latency        2.57ms     2.29ms   198.70ms
    HTTP codes:
      1xx - 0, 2xx - 91748, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8252
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8252
    Throughput:     4.03MB/s
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
    Reqs/sec     22992.99    6857.22   65082.27
    Latency        2.17ms     1.86ms   157.53ms
    HTTP codes:
      1xx - 0, 2xx - 97074, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2926
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2926
    Throughput:     5.11MB/s
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
    Reqs/sec     71942.76    4265.61   85050.02
    Latency      691.84us   180.60us     7.95ms
    HTTP codes:
      1xx - 0, 2xx - 95613, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4387
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4387
    Throughput:    10.89MB/s
  ```


