## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73826` | `4837` | `83771` |
| **86%** | [Hyper Express](#hyper-express) | `63349` | `5324` | `79383` |
| **34%** | [Node (Default)](#node-default) | `25329` | `6674` | `46469` |
| **33%** | [Fastify](#fastify) | `24634` | `7615` | `36401` |
| **28%** | [Hono](#hono) | `20649` | `6206` | `30987` |
| **26%** | [Koa](#koa) | `19239` | `7568` | `62952` |
| **11%** | [Carbon](#carbon) | `8234` | `1391` | `10410` |
| **9%** | [Express](#express) | `6342` | `1007` | `8376` |


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
    Reqs/sec      9021.59    5786.54   60575.48
    Latency        5.53ms     4.19ms   362.83ms
    HTTP codes:
      1xx - 0, 2xx - 90046, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9954
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9954
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
    Reqs/sec      7014.53    4656.03   58069.41
    Latency        7.12ms     3.87ms   356.02ms
    HTTP codes:
      1xx - 0, 2xx - 91595, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8405
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8405
    Throughput:     1.84MB/s
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
    Reqs/sec     23950.00    7052.12   36585.42
    Latency        2.09ms     2.06ms   183.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20089.63    5565.37   30238.58
    Latency        2.49ms     2.01ms   184.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.54MB/s
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
    Reqs/sec     62603.06    4559.26   68458.57
    Latency      796.63us    94.84us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     19393.53    7121.32   55903.85
    Latency        2.57ms     2.41ms   213.66ms
    HTTP codes:
      1xx - 0, 2xx - 93614, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6386
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6386
    Throughput:     4.11MB/s
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
    Reqs/sec     26466.33    7847.37   51837.01
    Latency        1.89ms     1.88ms   162.17ms
    HTTP codes:
      1xx - 0, 2xx - 96907, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3093
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3093
    Throughput:     5.87MB/s
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
    Reqs/sec     73220.55    5383.23   80608.33
    Latency      680.90us   160.07us     6.62ms
    HTTP codes:
      1xx - 0, 2xx - 98113, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1887
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1887
    Throughput:    11.36MB/s
  ```


