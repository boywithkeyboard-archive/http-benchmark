## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77890` | `3527` | `84602` |
| **87%** | [Hyper Express](#hyper-express) | `67838` | `3875` | `72772` |
| **46%** | [Node (Default)](#node-default) | `35951` | `11425` | `82835` |
| **41%** | [Fastify](#fastify) | `31577` | `9095` | `50575` |
| **38%** | [Koa](#koa) | `29934` | `13892` | `77229` |
| **36%** | [Hono](#hono) | `28281` | `7963` | `46589` |
| **12%** | [Carbon](#carbon) | `9262` | `2311` | `13304` |
| **9%** | [Express](#express) | `7106` | `1593` | `10116` |


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
    Reqs/sec     10527.32    7912.28   84224.35
    Latency        4.75ms     4.46ms   379.53ms
    HTTP codes:
      1xx - 0, 2xx - 89695, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10305
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10305
    Throughput:     2.14MB/s
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
    Reqs/sec      8295.38    7398.93   73755.86
    Latency        6.02ms     3.80ms   348.45ms
    HTTP codes:
      1xx - 0, 2xx - 88074, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11926
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11926
    Throughput:     2.09MB/s
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
    Reqs/sec     32811.84    9510.78   50905.40
    Latency        1.52ms     1.80ms   159.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.43MB/s
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
    Reqs/sec     29100.90    8679.88   45941.78
    Latency        1.72ms     2.19ms   190.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.57MB/s
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
    Reqs/sec     68704.39    3382.42   72264.43
    Latency      726.10us    77.55us     2.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.76MB/s
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
    Reqs/sec     31104.96   13519.37   83958.33
    Latency        1.60ms     2.26ms   198.29ms
    HTTP codes:
      1xx - 0, 2xx - 91409, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8591
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8591
    Throughput:     6.43MB/s
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
    Reqs/sec     33897.48    9193.92   72761.26
    Latency        1.47ms     1.73ms   141.67ms
    HTTP codes:
      1xx - 0, 2xx - 96749, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3251
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3251
    Throughput:     7.52MB/s
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
    Reqs/sec     78371.35    2797.64   82941.13
    Latency      635.64us   142.49us     7.00ms
    HTTP codes:
      1xx - 0, 2xx - 96820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3180
    Throughput:    12.01MB/s
  ```


