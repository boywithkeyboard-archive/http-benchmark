## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74223` | `6781` | `98234` |
| **85%** | [Hyper Express](#hyper-express) | `63056` | `4424` | `75191` |
| **35%** | [Node (Default)](#node-default) | `26315` | `7841` | `40659` |
| **33%** | [Fastify](#fastify) | `24465` | `7422` | `35554` |
| **26%** | [Hono](#hono) | `19269` | `5146` | `29911` |
| **24%** | [Koa](#koa) | `17855` | `6655` | `53642` |
| **11%** | [Carbon](#carbon) | `8122` | `1362` | `10490` |
| **9%** | [Express](#express) | `6410` | `1002` | `8354` |


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
    Reqs/sec      8496.73    3613.47   62034.00
    Latency        5.87ms     4.09ms   357.44ms
    HTTP codes:
      1xx - 0, 2xx - 95457, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4543
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4543
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
    Reqs/sec      6421.06    1007.63    8415.69
    Latency        7.78ms     3.66ms   349.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     24020.66    7236.39   35528.02
    Latency        2.08ms     1.94ms   174.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20146.46    5439.74   29176.41
    Latency        2.48ms     1.92ms   174.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     63658.07    4077.53   73292.02
    Latency      783.44us    68.91us     2.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     17995.68    6432.55   54354.61
    Latency        2.77ms     2.49ms   221.10ms
    HTTP codes:
      1xx - 0, 2xx - 94241, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5759
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5759
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
    Reqs/sec     26764.72    8171.85   46853.95
    Latency        1.86ms     1.98ms   170.56ms
    HTTP codes:
      1xx - 0, 2xx - 98047, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1953
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1953
    Throughput:     6.01MB/s
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
    Reqs/sec     74015.65    6664.90   87826.32
    Latency      673.12us   215.79us    12.09ms
    HTTP codes:
      1xx - 0, 2xx - 97600, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2400
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2400
    Throughput:    11.43MB/s
  ```


