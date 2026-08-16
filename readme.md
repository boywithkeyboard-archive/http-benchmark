## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71234` | `5515` | `90638` |
| **81%** | [Hyper Express](#hyper-express) | `57654` | `4251` | `62863` |
| **31%** | [Hono](#hono) | `22429` | `7017` | `30920` |
| **29%** | [Fastify](#fastify) | `20568` | `5084` | `37440` |
| **29%** | [Node (Default)](#node-default) | `20448` | `6038` | `72741` |
| **27%** | [Koa](#koa) | `19047` | `9051` | `78550` |
| **10%** | [Carbon](#carbon) | `7367` | `1247` | `10389` |
| **9%** | [Express](#express) | `6200` | `1082` | `8320` |


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
    Reqs/sec      8108.11    5764.63   64819.10
    Latency        6.15ms     4.58ms   388.85ms
    HTTP codes:
      1xx - 0, 2xx - 90766, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9234
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9234
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
    Reqs/sec      6143.07    1111.37    8288.87
    Latency        8.13ms     3.91ms   371.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     20082.38    4191.12   35253.11
    Latency        2.49ms     2.02ms   180.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     21196.64    6313.10   30396.77
    Latency        2.36ms     2.15ms   192.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     58422.51    3556.97   70231.94
    Latency        0.85ms    92.71us     4.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.30MB/s
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
    Reqs/sec     18399.71    9066.27   78864.78
    Latency        2.71ms     2.34ms   204.23ms
    HTTP codes:
      1xx - 0, 2xx - 92360, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7640
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7640
    Throughput:     3.84MB/s
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
    Reqs/sec     20388.54    4906.55   64440.57
    Latency        2.45ms     1.95ms   166.37ms
    HTTP codes:
      1xx - 0, 2xx - 97016, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2984
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2984
    Throughput:     4.53MB/s
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
    Reqs/sec     69787.54    4296.60   81528.66
    Latency      713.82us   203.96us     8.23ms
    HTTP codes:
      1xx - 0, 2xx - 96804, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3196
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3196
    Throughput:    10.69MB/s
  ```


