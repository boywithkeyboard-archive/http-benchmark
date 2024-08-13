## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74354` | `5478` | `84796` |
| **84%** | [Hyper Express](#hyper-express) | `62721` | `3943` | `72416` |
| **35%** | [Node (Default)](#node-default) | `25661` | `8369` | `53366` |
| **33%** | [Fastify](#fastify) | `24769` | `7672` | `35293` |
| **28%** | [Hono](#hono) | `20672` | `5803` | `29645` |
| **25%** | [Koa](#koa) | `18267` | `6218` | `55456` |
| **11%** | [Carbon](#carbon) | `8210` | `1373` | `10561` |
| **9%** | [Express](#express) | `6495` | `1036` | `8463` |


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
    Reqs/sec      8656.17    3445.40   48998.74
    Latency        5.77ms     4.22ms   363.49ms
    HTTP codes:
      1xx - 0, 2xx - 95734, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4266
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4266
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
    Reqs/sec      6442.86     994.83    8432.91
    Latency        7.76ms     3.65ms   346.85ms
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
    Reqs/sec     23528.66    7084.72   36227.18
    Latency        2.12ms     1.90ms   176.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     20358.61    5606.12   30488.70
    Latency        2.45ms     1.93ms   174.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     63157.09    4008.70   66771.04
    Latency      789.99us    72.31us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     17990.26    6672.89   51445.46
    Latency        2.77ms     2.35ms   213.65ms
    HTTP codes:
      1xx - 0, 2xx - 93663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6337
    Throughput:     3.81MB/s
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
    Reqs/sec     26231.73    7146.68   47088.23
    Latency        1.90ms     1.83ms   156.24ms
    HTTP codes:
      1xx - 0, 2xx - 98084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1916
    Throughput:     5.89MB/s
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
    Reqs/sec     74868.44    6400.76   80642.70
    Latency      664.77us   292.27us    11.67ms
    HTTP codes:
      1xx - 0, 2xx - 96399, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3601
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3601
    Throughput:    11.42MB/s
  ```


