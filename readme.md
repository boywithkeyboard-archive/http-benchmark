## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72423` | `5206` | `86754` |
| **86%** | [Hyper Express](#hyper-express) | `62162` | `3601` | `70508` |
| **37%** | [Node (Default)](#node-default) | `26894` | `8107` | `51450` |
| **35%** | [Fastify](#fastify) | `25264` | `7534` | `36093` |
| **30%** | [Hono](#hono) | `21491` | `6189` | `30239` |
| **26%** | [Koa](#koa) | `18543` | `6326` | `57355` |
| **12%** | [Carbon](#carbon) | `8331` | `1441` | `10399` |
| **9%** | [Express](#express) | `6448` | `1026` | `8309` |


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
    Reqs/sec      8815.58    4620.17   55127.80
    Latency        5.66ms     4.36ms   380.26ms
    HTTP codes:
      1xx - 0, 2xx - 93786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6214
    Throughput:     1.88MB/s
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
    Reqs/sec      7121.54    5370.95   60995.33
    Latency        7.01ms     4.02ms   369.83ms
    HTTP codes:
      1xx - 0, 2xx - 89551, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10449
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10449
    Throughput:     1.83MB/s
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
    Reqs/sec     23818.22    6936.77   35730.68
    Latency        2.10ms     2.04ms   184.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     21149.48    6345.07   31178.92
    Latency        2.36ms     2.04ms   181.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     62214.34    3829.61   66053.16
    Latency      800.93us    71.31us     2.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     19541.12    6867.95   52293.73
    Latency        2.55ms     2.39ms   207.42ms
    HTTP codes:
      1xx - 0, 2xx - 94104, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5896
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5896
    Throughput:     4.16MB/s
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
    Reqs/sec     26823.86    8496.46   49609.37
    Latency        1.86ms     1.86ms   163.13ms
    HTTP codes:
      1xx - 0, 2xx - 98011, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1989
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1989
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
    Reqs/sec     73800.60    5415.92   87923.78
    Latency      675.41us   146.80us     4.52ms
    HTTP codes:
      1xx - 0, 2xx - 97625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2375
    Throughput:    11.40MB/s
  ```


