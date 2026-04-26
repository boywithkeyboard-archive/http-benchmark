## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71495` | `4038` | `80424` |
| **83%** | [Hyper Express](#hyper-express) | `59044` | `3560` | `65422` |
| **30%** | [Hono](#hono) | `21757` | `6645` | `30073` |
| **29%** | [Fastify](#fastify) | `20420` | `5233` | `36079` |
| **28%** | [Node (Default)](#node-default) | `20305` | `6524` | `80183` |
| **25%** | [Koa](#koa) | `17601` | `9870` | `79281` |
| **10%** | [Carbon](#carbon) | `7266` | `1236` | `10168` |
| **9%** | [Express](#express) | `6169` | `1147` | `8214` |


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
    Reqs/sec      7783.45    5340.29   70452.26
    Latency        6.41ms     4.43ms   381.90ms
    HTTP codes:
      1xx - 0, 2xx - 91961, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8039
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8039
    Throughput:     1.63MB/s
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
    Reqs/sec      6070.09    1063.17    8124.46
    Latency        8.23ms     3.90ms   376.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     20867.82    5699.96   36368.86
    Latency        2.40ms     2.23ms   197.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     21015.86    6228.34   30385.93
    Latency        2.38ms     2.17ms   193.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     59766.02    4089.49   70331.26
    Latency      834.19us    81.98us     4.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     17539.63    8652.96   72823.04
    Latency        2.84ms     2.60ms   225.79ms
    HTTP codes:
      1xx - 0, 2xx - 92588, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7412
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7412
    Throughput:     3.67MB/s
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
    Reqs/sec     20162.57    5323.09   64491.98
    Latency        2.48ms     2.00ms   176.15ms
    HTTP codes:
      1xx - 0, 2xx - 97313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2687
    Throughput:     4.49MB/s
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
    Reqs/sec     71818.11    5462.46   86336.46
    Latency      693.17us   205.44us    10.04ms
    HTTP codes:
      1xx - 0, 2xx - 96505, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3495
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3495
    Throughput:    10.97MB/s
  ```


