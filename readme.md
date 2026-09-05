## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69361` | `3976` | `80208` |
| **84%** | [Hyper Express](#hyper-express) | `58353` | `3404` | `63924` |
| **30%** | [Hono](#hono) | `20985` | `6534` | `30566` |
| **29%** | [Node (Default)](#node-default) | `20448` | `4898` | `62676` |
| **28%** | [Fastify](#fastify) | `19358` | `4809` | `35943` |
| **26%** | [Koa](#koa) | `17741` | `8899` | `78212` |
| **10%** | [Carbon](#carbon) | `7263` | `1170` | `10386` |
| **9%** | [Express](#express) | `6269` | `1132` | `8302` |


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
    Reqs/sec      7782.12    5196.72   62926.57
    Latency        6.42ms     4.59ms   389.44ms
    HTTP codes:
      1xx - 0, 2xx - 92306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7694
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
    Reqs/sec      6273.90    1155.24    8606.08
    Latency        7.96ms     3.98ms   374.91ms
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
    Reqs/sec     19887.92    4923.74   35422.36
    Latency        2.51ms     1.92ms   179.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     21241.23    6216.57   30687.94
    Latency        2.35ms     2.27ms   199.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     58180.46    3193.57   64182.24
    Latency        0.86ms    92.32us     3.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.26MB/s
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
    Reqs/sec     19376.04    9003.35   77343.80
    Latency        2.57ms     2.48ms   216.11ms
    HTTP codes:
      1xx - 0, 2xx - 91853, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8147
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8147
    Throughput:     4.02MB/s
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
    Reqs/sec     20151.93    5141.26   57986.07
    Latency        2.47ms     2.04ms   174.64ms
    HTTP codes:
      1xx - 0, 2xx - 97427, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2573
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2573
    Throughput:     4.50MB/s
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
    Reqs/sec     70613.60    4518.44   82302.42
    Latency      705.65us   163.62us     8.21ms
    HTTP codes:
      1xx - 0, 2xx - 96892, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3108
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3108
    Throughput:    10.82MB/s
  ```


