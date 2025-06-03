## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72985` | `5140` | `81453` |
| **86%** | [Hyper Express](#hyper-express) | `62589` | `4496` | `77553` |
| **36%** | [Node (Default)](#node-default) | `26039` | `6966` | `50035` |
| **33%** | [Fastify](#fastify) | `24051` | `7061` | `35914` |
| **28%** | [Hono](#hono) | `20547` | `5771` | `30541` |
| **26%** | [Koa](#koa) | `18836` | `7515` | `60940` |
| **11%** | [Carbon](#carbon) | `8231` | `1373` | `10326` |
| **9%** | [Express](#express) | `6626` | `1124` | `8475` |


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
    Reqs/sec      8650.76    4533.29   55376.33
    Latency        5.77ms     4.32ms   371.45ms
    HTTP codes:
      1xx - 0, 2xx - 93425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6575
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
    Reqs/sec      6504.84    1053.75    8449.02
    Latency        7.68ms     3.65ms   350.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24763.14    7496.20   35573.20
    Latency        2.02ms     2.06ms   183.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     20270.85    5723.51   30468.95
    Latency        2.47ms     1.99ms   182.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     62065.24    3272.23   65813.35
    Latency      803.44us    67.63us     2.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18536.61    7696.23   64486.47
    Latency        2.69ms     2.41ms   211.01ms
    HTTP codes:
      1xx - 0, 2xx - 92258, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7742
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7742
    Throughput:     3.87MB/s
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
    Reqs/sec     26674.16    7346.55   45336.79
    Latency        1.87ms     1.85ms   161.19ms
    HTTP codes:
      1xx - 0, 2xx - 98141, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1859
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1859
    Throughput:     5.99MB/s
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
    Reqs/sec     74742.19    5756.08   90447.59
    Latency      666.49us   195.86us     8.18ms
    HTTP codes:
      1xx - 0, 2xx - 96682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3318
    Throughput:    11.44MB/s
  ```


