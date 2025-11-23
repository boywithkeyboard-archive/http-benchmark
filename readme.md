## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75417` | `2675` | `79855` |
| **84%** | [Hyper Express](#hyper-express) | `63162` | `3949` | `68106` |
| **33%** | [Node (Default)](#node-default) | `25118` | `7655` | `62330` |
| **31%** | [Fastify](#fastify) | `23540` | `6681` | `37649` |
| **28%** | [Hono](#hono) | `21483` | `6145` | `30816` |
| **24%** | [Koa](#koa) | `18434` | `7613` | `65594` |
| **11%** | [Carbon](#carbon) | `8346` | `1484` | `10483` |
| **8%** | [Express](#express) | `6333` | `1029` | `8299` |


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
    Reqs/sec      8800.05    5967.36   77776.71
    Latency        5.67ms     4.42ms   379.40ms
    HTTP codes:
      1xx - 0, 2xx - 91937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8063
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
    Reqs/sec      6249.58     997.12    8320.12
    Latency        8.00ms     3.66ms   351.95ms
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
    Reqs/sec     23493.18    7376.22   35374.61
    Latency        2.13ms     2.11ms   187.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     20846.99    5946.36   30685.88
    Latency        2.40ms     1.96ms   175.84ms
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
    Reqs/sec     62826.14    3897.81   66028.12
    Latency      793.50us    69.98us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     18045.62    7791.54   67005.67
    Latency        2.75ms     2.41ms   213.38ms
    HTTP codes:
      1xx - 0, 2xx - 93017, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6983
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6983
    Throughput:     3.81MB/s
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
    Reqs/sec     25809.02    8748.12   77147.99
    Latency        1.93ms     1.94ms   164.06ms
    HTTP codes:
      1xx - 0, 2xx - 96672, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3328
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3328
    Throughput:     5.71MB/s
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
    Reqs/sec     74979.07    2601.07   80252.71
    Latency      664.54us   121.51us     4.30ms
    HTTP codes:
      1xx - 0, 2xx - 97497, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2503
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2503
    Throughput:    11.57MB/s
  ```


