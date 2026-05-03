## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73175` | `4693` | `84645` |
| **83%** | [Hyper Express](#hyper-express) | `60421` | `3785` | `67159` |
| **30%** | [Hono](#hono) | `21742` | `6858` | `30695` |
| **28%** | [Node (Default)](#node-default) | `20852` | `6881` | `78080` |
| **28%** | [Fastify](#fastify) | `20157` | `5239` | `36793` |
| **25%** | [Koa](#koa) | `18233` | `8918` | `78041` |
| **10%** | [Carbon](#carbon) | `7145` | `1161` | `10138` |
| **8%** | [Express](#express) | `6062` | `1070` | `8169` |


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
    Reqs/sec      8068.21    6270.01   72996.84
    Latency        6.18ms     4.67ms   394.15ms
    HTTP codes:
      1xx - 0, 2xx - 90253, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9747
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9747
    Throughput:     1.66MB/s
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
    Reqs/sec      6060.37    1050.51    8094.80
    Latency        8.25ms     3.90ms   371.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     21381.83    5791.17   36957.72
    Latency        2.34ms     2.11ms   191.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     21367.44    6510.42   29867.10
    Latency        2.34ms     2.16ms   192.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     59606.93    3530.27   67803.46
    Latency      836.64us    85.06us     3.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.47MB/s
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
    Reqs/sec     18795.71    8684.56   65286.67
    Latency        2.65ms     2.57ms   223.21ms
    HTTP codes:
      1xx - 0, 2xx - 93212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6788
    Throughput:     3.96MB/s
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
    Reqs/sec     20149.60    5853.66   74729.30
    Latency        2.48ms     2.09ms   175.16ms
    HTTP codes:
      1xx - 0, 2xx - 96332, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3668
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3668
    Throughput:     4.44MB/s
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
    Reqs/sec     71213.28    5323.64   90054.83
    Latency      698.34us   170.31us     7.47ms
    HTTP codes:
      1xx - 0, 2xx - 96815, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3185
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3185
    Throughput:    10.92MB/s
  ```


