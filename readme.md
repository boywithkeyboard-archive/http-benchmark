## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `101218` | `2948` | `104649` |
| **87%** | [Hyper Express](#hyper-express) | `88186` | `4091` | `92151` |
| **45%** | [Node (Default)](#node-default) | `45398` | `14738` | `104748` |
| **44%** | [Fastify](#fastify) | `44044` | `13974` | `65420` |
| **31%** | [Koa](#koa) | `30878` | `13113` | `98239` |
| **30%** | [Hono](#hono) | `30743` | `8856` | `57830` |
| **11%** | [Carbon](#carbon) | `11546` | `2337` | `17272` |
| **9%** | [Express](#express) | `9475` | `1921` | `13437` |


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
    Reqs/sec     12417.82    7947.18   98015.68
    Latency        4.01ms     3.33ms   285.29ms
    HTTP codes:
      1xx - 0, 2xx - 92037, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7963
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7963
    Throughput:     2.60MB/s
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
    Reqs/sec     10276.34    8893.10   94028.40
    Latency        4.85ms     2.92ms   269.37ms
    HTTP codes:
      1xx - 0, 2xx - 89264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10736
    Throughput:     2.63MB/s
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
    Reqs/sec     48254.54   19655.78  114174.30
    Latency        1.03ms     1.52ms   129.71ms
    HTTP codes:
      1xx - 0, 2xx - 81191, 3xx - 0, 4xx - 0, 5xx - 0
      others - 18809
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 18809
    Throughput:     8.89MB/s
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
    Reqs/sec     34722.52   14918.16  108050.46
    Latency        1.43ms     1.69ms   142.11ms
    HTTP codes:
      1xx - 0, 2xx - 90486, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9514
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9514
    Throughput:     7.12MB/s
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
    Reqs/sec     89050.79    6140.37  110308.45
    Latency      559.82us   237.62us    10.81ms
    HTTP codes:
      1xx - 0, 2xx - 92802, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7198
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7198
    Throughput:    11.73MB/s
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
    Reqs/sec     32553.25   13506.88   95280.82
    Latency        1.53ms     1.82ms   153.35ms
    HTTP codes:
      1xx - 0, 2xx - 91752, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8248
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8248
    Throughput:     6.76MB/s
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
    Reqs/sec     49771.66   16128.59   84323.07
    Latency        1.00ms     1.40ms   113.29ms
    HTTP codes:
      1xx - 0, 2xx - 97157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2843
    Throughput:    11.06MB/s
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
    Reqs/sec     97182.41    6607.62  119758.20
    Latency      512.07us   218.84us     7.33ms
    HTTP codes:
      1xx - 0, 2xx - 92878, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7122
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7122
    Throughput:    14.28MB/s
  ```


