## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75365` | `2755` | `80858` |
| **86%** | [Hyper Express](#hyper-express) | `64461` | `3095` | `68788` |
| **36%** | [Node (Default)](#node-default) | `27248` | `8979` | `75014` |
| **33%** | [Fastify](#fastify) | `24520` | `7641` | `35552` |
| **28%** | [Hono](#hono) | `20760` | `5390` | `29692` |
| **25%** | [Koa](#koa) | `19153` | `9158` | `81886` |
| **11%** | [Carbon](#carbon) | `8343` | `1466` | `10414` |
| **9%** | [Express](#express) | `6557` | `1104` | `8511` |


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
    Reqs/sec      8886.93    5785.62   69516.32
    Latency        5.62ms     4.31ms   373.31ms
    HTTP codes:
      1xx - 0, 2xx - 91964, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8036
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8036
    Throughput:     1.86MB/s
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
    Reqs/sec      7160.36    5684.27   81223.22
    Latency        6.97ms     3.86ms   355.66ms
    HTTP codes:
      1xx - 0, 2xx - 90790, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9210
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9210
    Throughput:     1.86MB/s
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
    Reqs/sec     24368.68    7330.06   36793.08
    Latency        2.05ms     2.06ms   183.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     21607.43    6201.01   31143.39
    Latency        2.31ms     2.05ms   181.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     62917.40    3947.38   68590.75
    Latency      792.84us    68.13us     2.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     20827.25   10191.63   78773.49
    Latency        2.40ms     2.40ms   211.92ms
    HTTP codes:
      1xx - 0, 2xx - 90040, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9960
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9960
    Throughput:     4.24MB/s
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
    Reqs/sec     27009.15    8983.24   63826.14
    Latency        1.85ms     1.91ms   163.53ms
    HTTP codes:
      1xx - 0, 2xx - 97326, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2674
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2674
    Throughput:     6.02MB/s
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
    Reqs/sec     74436.32    2873.46   81268.96
    Latency      668.94us   173.25us    10.73ms
    HTTP codes:
      1xx - 0, 2xx - 96343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3657
    Throughput:    11.35MB/s
  ```


