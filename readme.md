## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72850` | `4391` | `77346` |
| **84%** | [Hyper Express](#hyper-express) | `61025` | `3387` | `63879` |
| **35%** | [Node (Default)](#node-default) | `25822` | `7590` | `57263` |
| **32%** | [Fastify](#fastify) | `23639` | `6964` | `35586` |
| **28%** | [Hono](#hono) | `20434` | `5678` | `29785` |
| **27%** | [Koa](#koa) | `19790` | `7852` | `56705` |
| **11%** | [Carbon](#carbon) | `8292` | `1392` | `10467` |
| **9%** | [Express](#express) | `6603` | `1103` | `8462` |


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
    Reqs/sec      8832.11    4280.91   53177.17
    Latency        5.65ms     4.23ms   368.16ms
    HTTP codes:
      1xx - 0, 2xx - 94190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5810
    Throughput:     1.89MB/s
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
    Reqs/sec      7043.26    4735.35   56684.04
    Latency        7.09ms     3.96ms   360.94ms
    HTTP codes:
      1xx - 0, 2xx - 91442, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8558
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8558
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
    Reqs/sec     24515.73    7569.50   36003.34
    Latency        2.04ms     2.04ms   183.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     21223.59    5744.68   30289.66
    Latency        2.35ms     2.06ms   183.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     61412.55    3306.14   64472.55
    Latency      812.14us    68.66us     3.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18586.48    6729.06   52624.78
    Latency        2.69ms     2.40ms   209.00ms
    HTTP codes:
      1xx - 0, 2xx - 93154, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6846
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6846
    Throughput:     3.91MB/s
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
    Reqs/sec     26065.83    7333.97   57375.58
    Latency        1.91ms     1.85ms   162.00ms
    HTTP codes:
      1xx - 0, 2xx - 97464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2536
    Throughput:     5.82MB/s
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
    Reqs/sec     72527.51    5066.26   80404.82
    Latency      687.02us   166.56us     7.66ms
    HTTP codes:
      1xx - 0, 2xx - 97504, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2496
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2496
    Throughput:    11.19MB/s
  ```


