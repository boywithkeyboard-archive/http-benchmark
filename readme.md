## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `103209` | `3425` | `110611` |
| **86%** | [Hyper Express](#hyper-express) | `89238` | `4318` | `99908` |
| **47%** | [Node (Default)](#node-default) | `48161` | `15247` | `107467` |
| **44%** | [Fastify](#fastify) | `45452` | `12707` | `67638` |
| **41%** | [Hono](#hono) | `41814` | `13632` | `62321` |
| **38%** | [Koa](#koa) | `39669` | `16091` | `97827` |
| **11%** | [Carbon](#carbon) | `11643` | `2669` | `17299` |
| **9%** | [Express](#express) | `9787` | `2089` | `13678` |


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
    Reqs/sec     13888.76   10927.60  110583.51
    Latency        3.59ms     3.40ms   290.14ms
    HTTP codes:
      1xx - 0, 2xx - 88119, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11881
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11881
    Throughput:     2.78MB/s
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
    Reqs/sec     10112.44    8047.56   95549.84
    Latency        4.94ms     2.86ms   261.20ms
    HTTP codes:
      1xx - 0, 2xx - 90559, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9441
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9439
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     2.62MB/s
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
    Reqs/sec     52244.11   17755.99   69909.17
    Latency        0.96ms     1.47ms   129.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    11.84MB/s
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
    Reqs/sec     40132.59   12677.84   63232.46
    Latency        1.24ms     1.53ms   131.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     91109.42    4185.90   95554.59
    Latency      547.28us    45.68us     2.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.94MB/s
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
    Reqs/sec     40984.82   15463.65   98758.36
    Latency        1.22ms     1.66ms   140.90ms
    HTTP codes:
      1xx - 0, 2xx - 92694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7306
    Throughput:     8.59MB/s
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
    Reqs/sec     49435.50   14637.51   88261.44
    Latency        1.01ms     1.43ms   118.92ms
    HTTP codes:
      1xx - 0, 2xx - 96600, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3400
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3400
    Throughput:    10.91MB/s
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
    Reqs/sec    105730.77    5249.73  124608.80
    Latency      470.27us   163.45us     9.03ms
    HTTP codes:
      1xx - 0, 2xx - 96124, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3876
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3876
    Throughput:    16.10MB/s
  ```


