## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75419` | `6868` | `90740` |
| **86%** | [Hyper Express](#hyper-express) | `64500` | `3270` | `74241` |
| **36%** | [Node (Default)](#node-default) | `26824` | `7553` | `49596` |
| **32%** | [Fastify](#fastify) | `24457` | `7269` | `36561` |
| **28%** | [Hono](#hono) | `20779` | `5369` | `29508` |
| **24%** | [Koa](#koa) | `18249` | `6431` | `55705` |
| **11%** | [Carbon](#carbon) | `8379` | `1452` | `10689` |
| **9%** | [Express](#express) | `6674` | `1105` | `8583` |


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
    Reqs/sec      8756.01    3597.18   48903.44
    Latency        5.69ms     4.15ms   361.65ms
    HTTP codes:
      1xx - 0, 2xx - 95469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4531
    Throughput:     1.90MB/s
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
    Reqs/sec      7000.50    3844.18   51043.53
    Latency        7.13ms     3.77ms   343.73ms
    HTTP codes:
      1xx - 0, 2xx - 93208, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6792
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6792
    Throughput:     1.87MB/s
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
    Reqs/sec     24303.24    7170.66   35344.08
    Latency        2.06ms     1.94ms   176.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21101.16    5814.43   29518.13
    Latency        2.37ms     1.94ms   174.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     64600.30    4199.98   76454.06
    Latency      771.61us    68.16us     4.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.18MB/s
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
    Reqs/sec     18956.92    7441.38   57733.03
    Latency        2.63ms     2.32ms   200.08ms
    HTTP codes:
      1xx - 0, 2xx - 91504, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8496
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8496
    Throughput:     3.92MB/s
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
    Reqs/sec     25780.03    7445.75   50522.20
    Latency        1.94ms     1.90ms   164.05ms
    HTTP codes:
      1xx - 0, 2xx - 97973, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2027
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2027
    Throughput:     5.77MB/s
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
    Reqs/sec     74037.52    4929.48   81544.89
    Latency      672.71us   190.59us    11.96ms
    HTTP codes:
      1xx - 0, 2xx - 97410, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2590
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2590
    Throughput:    11.41MB/s
  ```


