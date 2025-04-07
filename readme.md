## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73923` | `5444` | `88306` |
| **84%** | [Hyper Express](#hyper-express) | `62453` | `4090` | `66251` |
| **34%** | [Node (Default)](#node-default) | `25361` | `7541` | `60124` |
| **33%** | [Fastify](#fastify) | `24464` | `7255` | `35863` |
| **28%** | [Hono](#hono) | `20398` | `5689` | `29532` |
| **25%** | [Koa](#koa) | `18487` | `6662` | `51265` |
| **11%** | [Carbon](#carbon) | `8306` | `1426` | `10434` |
| **9%** | [Express](#express) | `6538` | `1085` | `8551` |


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
    Reqs/sec      8989.87    5083.61   55822.93
    Latency        5.55ms     4.35ms   377.08ms
    HTTP codes:
      1xx - 0, 2xx - 91764, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8236
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8236
    Throughput:     1.87MB/s
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
    Reqs/sec      7112.76    4798.64   59494.26
    Latency        7.02ms     3.84ms   352.12ms
    HTTP codes:
      1xx - 0, 2xx - 90824, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9176
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9176
    Throughput:     1.85MB/s
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
    Reqs/sec     24775.78    7581.07   36569.14
    Latency        2.02ms     1.94ms   175.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20854.02    5993.98   30471.50
    Latency        2.40ms     2.01ms   181.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62227.59    3586.03   65512.06
    Latency      801.12us    67.69us     3.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18927.11    6725.44   53585.18
    Latency        2.64ms     2.35ms   206.32ms
    HTTP codes:
      1xx - 0, 2xx - 93471, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6529
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6529
    Throughput:     4.00MB/s
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
    Reqs/sec     27628.21    8986.94   58943.55
    Latency        1.81ms     1.85ms   160.62ms
    HTTP codes:
      1xx - 0, 2xx - 97269, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2731
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2731
    Throughput:     6.14MB/s
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
    Reqs/sec     74554.25    5419.75   83993.83
    Latency      669.02us   166.29us     5.17ms
    HTTP codes:
      1xx - 0, 2xx - 96982, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3018
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3018
    Throughput:    11.43MB/s
  ```


