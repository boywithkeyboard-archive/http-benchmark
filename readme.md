## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75316` | `3470` | `81600` |
| **84%** | [Hyper Express](#hyper-express) | `63004` | `3699` | `68230` |
| **36%** | [Node (Default)](#node-default) | `26795` | `8419` | `70603` |
| **31%** | [Fastify](#fastify) | `23334` | `7123` | `35325` |
| **27%** | [Hono](#hono) | `20358` | `5728` | `30051` |
| **25%** | [Koa](#koa) | `19120` | `8924` | `75269` |
| **11%** | [Carbon](#carbon) | `8376` | `1497` | `10487` |
| **9%** | [Express](#express) | `6603` | `1161` | `8435` |


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
    Reqs/sec      9132.10    6312.57   73944.63
    Latency        5.46ms     4.21ms   362.80ms
    HTTP codes:
      1xx - 0, 2xx - 91287, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8713
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8713
    Throughput:     1.90MB/s
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
    Reqs/sec      6961.88    5728.19   77480.55
    Latency        7.18ms     3.90ms   360.61ms
    HTTP codes:
      1xx - 0, 2xx - 90669, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9331
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9331
    Throughput:     1.81MB/s
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
    Reqs/sec     25231.75    7935.44   35843.14
    Latency        1.98ms     2.04ms   182.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.72MB/s
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
    Reqs/sec     20608.33    5207.50   29367.67
    Latency        2.42ms     1.99ms   178.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62791.88    4748.50   90227.11
    Latency      798.38us    73.54us     3.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     19595.08    9201.86   78775.48
    Latency        2.55ms     2.33ms   204.73ms
    HTTP codes:
      1xx - 0, 2xx - 90994, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9006
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9006
    Throughput:     4.03MB/s
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
    Reqs/sec     26134.49    8420.65   82397.73
    Latency        1.91ms     1.86ms   161.84ms
    HTTP codes:
      1xx - 0, 2xx - 96271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3729
    Throughput:     5.76MB/s
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
    Reqs/sec     76174.25    4922.61  109329.70
    Latency      658.37us   120.94us     5.71ms
    HTTP codes:
      1xx - 0, 2xx - 97515, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2485
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2485
    Throughput:    11.68MB/s
  ```


