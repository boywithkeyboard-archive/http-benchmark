## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `121627` | `4954` | `129750` |
| **79%** | [Hyper Express](#hyper-express) | `96300` | `7901` | `100733` |
| **30%** | [Node (Default)](#node-default) | `36806` | `11285` | `101990` |
| **29%** | [Fastify](#fastify) | `34852` | `7899` | `54823` |
| **26%** | [Koa](#koa) | `31549` | `13428` | `102433` |
| **26%** | [Hono](#hono) | `31113` | `6920` | `47153` |
| **10%** | [Carbon](#carbon) | `11692` | `2259` | `16330` |
| **7%** | [Express](#express) | `8611` | `1706` | `11759` |


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
    Reqs/sec     13820.34   13397.48  127185.63
    Latency        3.61ms     3.72ms   319.16ms
    HTTP codes:
      1xx - 0, 2xx - 84633, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15367
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15367
    Throughput:     2.66MB/s
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
    Reqs/sec      9496.62    9175.65   98285.81
    Latency        5.26ms     3.50ms   321.93ms
    HTTP codes:
      1xx - 0, 2xx - 88116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11884
    Throughput:     2.40MB/s
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
    Reqs/sec     34196.33    8046.23   54924.73
    Latency        1.46ms     1.53ms   133.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.75MB/s
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
    Reqs/sec     30344.50    7189.69   47160.51
    Latency        1.65ms     1.49ms   130.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.86MB/s
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
    Reqs/sec     95548.21    6286.68   99379.95
    Latency      521.54us    53.96us     2.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    13.57MB/s
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
    Reqs/sec     31219.70   14910.35  120215.83
    Latency        1.60ms     1.70ms   146.30ms
    HTTP codes:
      1xx - 0, 2xx - 89221, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10779
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10779
    Throughput:     6.30MB/s
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
    Reqs/sec     37172.84   10255.10  101525.24
    Latency        1.34ms     1.38ms   102.97ms
    HTTP codes:
      1xx - 0, 2xx - 96500, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3500
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3500
    Throughput:     8.23MB/s
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
    Reqs/sec    119160.05    5749.32  133000.98
    Latency      418.36us   123.00us     5.81ms
    HTTP codes:
      1xx - 0, 2xx - 96549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3451
    Throughput:    18.16MB/s
  ```


