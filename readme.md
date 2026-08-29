## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71090` | `3459` | `80620` |
| **81%** | [Hyper Express](#hyper-express) | `57362` | `4413` | `67478` |
| **31%** | [Node (Default)](#node-default) | `21768` | `6304` | `62459` |
| **31%** | [Hono](#hono) | `21765` | `6431` | `29393` |
| **29%** | [Fastify](#fastify) | `20648` | `6083` | `35660` |
| **28%** | [Koa](#koa) | `19959` | `9087` | `76679` |
| **10%** | [Carbon](#carbon) | `7429` | `1223` | `10226` |
| **9%** | [Express](#express) | `6086` | `1054` | `8015` |


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
    Reqs/sec      8009.96    5186.73   67207.59
    Latency        6.22ms     4.68ms   398.43ms
    HTTP codes:
      1xx - 0, 2xx - 91888, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8112
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8112
    Throughput:     1.67MB/s
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
    Reqs/sec      6180.77    1056.77    8139.56
    Latency        8.09ms     3.92ms   372.65ms
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
    Reqs/sec     21911.42    6146.53   34876.71
    Latency        2.28ms     2.14ms   190.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.97MB/s
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
    Reqs/sec     21882.26    6378.96   29341.98
    Latency        2.28ms     2.27ms   203.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     59632.98    3583.62   65561.22
    Latency      836.06us    94.97us     4.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.47MB/s
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
    Reqs/sec     19818.20    8261.28   64426.77
    Latency        2.52ms     2.50ms   216.98ms
    HTTP codes:
      1xx - 0, 2xx - 92388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7612
    Throughput:     4.14MB/s
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
    Reqs/sec     21384.40    6368.25   68823.59
    Latency        2.34ms     2.04ms   177.60ms
    HTTP codes:
      1xx - 0, 2xx - 96234, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3766
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3766
    Throughput:     4.70MB/s
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
    Reqs/sec     70988.73    4076.04   83338.46
    Latency      701.08us   144.40us     7.28ms
    HTTP codes:
      1xx - 0, 2xx - 97159, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2841
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2841
    Throughput:    10.92MB/s
  ```


