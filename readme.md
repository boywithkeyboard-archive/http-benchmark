## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `135061` | `6266` | `142972` |
| **84%** | [Hyper Express](#hyper-express) | `114055` | `11125` | `120787` |
| **36%** | [Node (Default)](#node-default) | `48023` | `11884` | `100933` |
| **32%** | [Fastify](#fastify) | `43889` | `8918` | `68375` |
| **29%** | [Hono](#hono) | `38848` | `10573` | `62316` |
| **28%** | [Koa](#koa) | `38427` | `17171` | `130628` |
| **13%** | [Carbon](#carbon) | `17217` | `4479` | `24668` |
| **8%** | [Express](#express) | `11375` | `2218` | `16398` |


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
    Reqs/sec     20356.48   17932.41  134117.03
    Latency        2.45ms     3.76ms   301.70ms
    HTTP codes:
      1xx - 0, 2xx - 82502, 3xx - 0, 4xx - 0, 5xx - 0
      others - 17498
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 17498
    Throughput:     3.81MB/s
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
    Reqs/sec     13300.69   13146.14  122633.62
    Latency        3.75ms     2.94ms   266.62ms
    HTTP codes:
      1xx - 0, 2xx - 85081, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14919
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14919
    Throughput:     3.24MB/s
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
    Reqs/sec     50279.09   20587.24  125979.93
    Latency        0.99ms     1.31ms   101.86ms
    HTTP codes:
      1xx - 0, 2xx - 78590, 3xx - 0, 4xx - 0, 5xx - 0
      others - 21410
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 21410
    Throughput:     8.96MB/s
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
    Reqs/sec     42252.70   18567.01  131678.77
    Latency        1.18ms     1.26ms    98.09ms
    HTTP codes:
      1xx - 0, 2xx - 87118, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12882
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12882
    Throughput:     8.34MB/s
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
    Reqs/sec    114241.67    6608.74  119871.81
    Latency      434.83us   188.36us     9.05ms
    HTTP codes:
      1xx - 0, 2xx - 90136, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9864
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9864
    Throughput:    14.62MB/s
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
    Reqs/sec     37822.88   17675.55  135157.75
    Latency        1.32ms     1.40ms   117.38ms
    HTTP codes:
      1xx - 0, 2xx - 87014, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12986
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12986
    Throughput:     7.44MB/s
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
    Reqs/sec     47387.99   11033.42  112676.58
    Latency        1.05ms     1.11ms    92.02ms
    HTTP codes:
      1xx - 0, 2xx - 95571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4429
    Throughput:    10.40MB/s
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
    Reqs/sec    135366.82    6193.88  142190.70
    Latency      367.03us   131.06us     5.12ms
    HTTP codes:
      1xx - 0, 2xx - 94312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5688
    Throughput:    20.20MB/s
  ```


