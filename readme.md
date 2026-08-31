## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68538` | `3532` | `79455` |
| **84%** | [Hyper Express](#hyper-express) | `57627` | `3774` | `64051` |
| **29%** | [Hono](#hono) | `19708` | `6492` | `30450` |
| **29%** | [Koa](#koa) | `19552` | `9487` | `77053` |
| **28%** | [Fastify](#fastify) | `19424` | `3954` | `33503` |
| **28%** | [Node (Default)](#node-default) | `19383` | `5096` | `66671` |
| **11%** | [Carbon](#carbon) | `7270` | `1251` | `10332` |
| **9%** | [Express](#express) | `6038` | `1058` | `8248` |


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
    Reqs/sec      7857.26    5369.75   64235.38
    Latency        6.35ms     4.71ms   396.12ms
    HTTP codes:
      1xx - 0, 2xx - 91360, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8640
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8640
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
    Reqs/sec      6301.94    1186.64    8240.98
    Latency        7.93ms     3.90ms   371.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     20766.59    6269.35   35696.15
    Latency        2.41ms     2.20ms   194.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     21042.33    6547.02   29728.62
    Latency        2.37ms     2.31ms   205.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     57247.67    3121.03   60011.39
    Latency        0.87ms    93.57us     3.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.13MB/s
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
    Reqs/sec     18660.50    8498.76   71017.73
    Latency        2.67ms     2.41ms   209.62ms
    HTTP codes:
      1xx - 0, 2xx - 92493, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7507
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7507
    Throughput:     3.90MB/s
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
    Reqs/sec     20255.50    4928.27   60814.77
    Latency        2.47ms     1.99ms   175.20ms
    HTTP codes:
      1xx - 0, 2xx - 97087, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2913
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2913
    Throughput:     4.49MB/s
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
    Reqs/sec     69364.46    4703.28   84304.27
    Latency      718.94us   202.35us    10.84ms
    HTTP codes:
      1xx - 0, 2xx - 96494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3506
    Throughput:    10.57MB/s
  ```


