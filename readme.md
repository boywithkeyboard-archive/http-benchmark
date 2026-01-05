## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75376` | `3154` | `78412` |
| **82%** | [Hyper Express](#hyper-express) | `61766` | `3623` | `64865` |
| **31%** | [Node (Default)](#node-default) | `23242` | `6772` | `79681` |
| **28%** | [Fastify](#fastify) | `21109` | `5526` | `35540` |
| **24%** | [Hono](#hono) | `18396` | `4052` | `29177` |
| **22%** | [Koa](#koa) | `16901` | `8679` | `82078` |
| **11%** | [Carbon](#carbon) | `7981` | `1384` | `10259` |
| **8%** | [Express](#express) | `6273` | `1008` | `8349` |


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
    Reqs/sec      8584.01    6857.12   76794.85
    Latency        5.81ms     4.32ms   372.70ms
    HTTP codes:
      1xx - 0, 2xx - 89487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10513
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
    Reqs/sec      6299.34    1047.43    8317.12
    Latency        7.94ms     3.76ms   356.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     21090.70    4463.60   34690.99
    Latency        2.37ms     1.89ms   173.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     17990.65    3996.83   29252.98
    Latency        2.78ms     2.02ms   179.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.07MB/s
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
    Reqs/sec     62740.28    3358.89   66366.58
    Latency      794.70us    66.58us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     16556.47    7774.14   72484.92
    Latency        3.01ms     2.40ms   213.08ms
    HTTP codes:
      1xx - 0, 2xx - 92076, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7924
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7924
    Throughput:     3.45MB/s
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
    Reqs/sec     22540.53    6562.53   71936.39
    Latency        2.21ms     2.04ms   175.95ms
    HTTP codes:
      1xx - 0, 2xx - 96264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3736
    Throughput:     4.97MB/s
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
    Reqs/sec     74876.47    2967.41   85828.74
    Latency      664.72us   157.94us     7.02ms
    HTTP codes:
      1xx - 0, 2xx - 96885, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3115
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3115
    Throughput:    11.49MB/s
  ```


