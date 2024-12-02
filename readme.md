## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73793` | `6465` | `87982` |
| **85%** | [Hyper Express](#hyper-express) | `62675` | `4656` | `83475` |
| **35%** | [Node (Default)](#node-default) | `25780` | `7109` | `53611` |
| **34%** | [Fastify](#fastify) | `25327` | `8067` | `36662` |
| **29%** | [Hono](#hono) | `21474` | `6165` | `30014` |
| **25%** | [Koa](#koa) | `18349` | `6312` | `62315` |
| **11%** | [Carbon](#carbon) | `8151` | `1383` | `10443` |
| **9%** | [Express](#express) | `6577` | `1084` | `8611` |


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
    Reqs/sec      8573.35    3985.96   58223.84
    Latency        5.82ms     4.22ms   369.24ms
    HTTP codes:
      1xx - 0, 2xx - 94682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5318
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
    Reqs/sec      6543.10    1017.39    8475.82
    Latency        7.64ms     3.57ms   341.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     23735.68    6906.36   35781.27
    Latency        2.10ms     1.97ms   174.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     20567.78    5738.50   29822.96
    Latency        2.43ms     1.92ms   173.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     62565.18    3699.89   68241.42
    Latency      796.88us    76.41us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18932.55    6244.18   46667.20
    Latency        2.63ms     2.34ms   203.03ms
    HTTP codes:
      1xx - 0, 2xx - 95239, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4761
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4761
    Throughput:     4.08MB/s
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
    Reqs/sec     26229.85    7386.89   44296.88
    Latency        1.90ms     1.93ms   165.39ms
    HTTP codes:
      1xx - 0, 2xx - 98115, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1885
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1885
    Throughput:     5.90MB/s
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
    Reqs/sec     73864.94    5861.69   80932.11
    Latency      674.92us   138.91us     9.77ms
    HTTP codes:
      1xx - 0, 2xx - 97972, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2028
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2028
    Throughput:    11.44MB/s
  ```


