## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74645` | `5012` | `85041` |
| **85%** | [Hyper Express](#hyper-express) | `63699` | `3778` | `67029` |
| **36%** | [Node (Default)](#node-default) | `26817` | `7949` | `57967` |
| **33%** | [Fastify](#fastify) | `24830` | `7409` | `36856` |
| **28%** | [Hono](#hono) | `21190` | `5674` | `30583` |
| **26%** | [Koa](#koa) | `19244` | `6878` | `53423` |
| **11%** | [Carbon](#carbon) | `8391` | `1451` | `10390` |
| **9%** | [Express](#express) | `6635` | `1096` | `8495` |


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
    Reqs/sec      8554.41    3720.77   50218.05
    Latency        5.83ms     4.22ms   368.62ms
    HTTP codes:
      1xx - 0, 2xx - 95389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4611
    Throughput:     1.85MB/s
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
    Reqs/sec      6996.50    4066.87   50806.37
    Latency        7.14ms     3.87ms   351.39ms
    HTTP codes:
      1xx - 0, 2xx - 92969, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7031
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7031
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
    Reqs/sec     24253.09    7344.63   36885.47
    Latency        2.06ms     2.05ms   183.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     21713.91    6121.84   30859.81
    Latency        2.30ms     2.09ms   187.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     62959.66    3895.31   72353.36
    Latency      791.53us    68.37us     3.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     19570.69    7168.68   61414.61
    Latency        2.55ms     2.36ms   205.75ms
    HTTP codes:
      1xx - 0, 2xx - 93159, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6841
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6841
    Throughput:     4.13MB/s
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
    Reqs/sec     27192.86    7952.23   52120.15
    Latency        1.84ms     1.81ms   155.10ms
    HTTP codes:
      1xx - 0, 2xx - 97128, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2872
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2872
    Throughput:     6.05MB/s
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
    Reqs/sec     74728.65    4896.28   88052.60
    Latency      665.53us   178.59us    11.28ms
    HTTP codes:
      1xx - 0, 2xx - 96616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3384
    Throughput:    11.43MB/s
  ```


