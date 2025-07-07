## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73876` | `4960` | `87371` |
| **84%** | [Hyper Express](#hyper-express) | `62064` | `3365` | `64359` |
| **34%** | [Node (Default)](#node-default) | `24883` | `7608` | `63731` |
| **32%** | [Fastify](#fastify) | `23788` | `7297` | `36040` |
| **28%** | [Hono](#hono) | `20603` | `5707` | `30005` |
| **24%** | [Koa](#koa) | `18044` | `6718` | `54048` |
| **11%** | [Carbon](#carbon) | `8221` | `1397` | `10305` |
| **9%** | [Express](#express) | `6406` | `1058` | `8357` |


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
    Reqs/sec      8524.44    3806.44   51393.02
    Latency        5.85ms     4.28ms   370.01ms
    HTTP codes:
      1xx - 0, 2xx - 94869, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5131
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5131
    Throughput:     1.84MB/s
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
    Reqs/sec      6474.59    1065.58    8370.82
    Latency        7.72ms     3.70ms   352.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23378.76    6676.71   36318.43
    Latency        2.14ms     2.06ms   185.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     21043.03    5744.64   30371.19
    Latency        2.37ms     2.00ms   179.93ms
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
    Reqs/sec     62175.33    3603.11   65864.89
    Latency      802.05us    73.24us     3.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19226.72    6953.96   55500.86
    Latency        2.60ms     2.47ms   216.53ms
    HTTP codes:
      1xx - 0, 2xx - 93735, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6265
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6265
    Throughput:     4.07MB/s
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
    Reqs/sec     25301.66    8020.69   51423.20
    Latency        1.97ms     1.92ms   169.73ms
    HTTP codes:
      1xx - 0, 2xx - 97799, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2201
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2201
    Throughput:     5.67MB/s
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
    Reqs/sec     73762.50    5393.51   93417.43
    Latency      677.87us   163.17us     6.40ms
    HTTP codes:
      1xx - 0, 2xx - 97301, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2699
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2699
    Throughput:    11.32MB/s
  ```


