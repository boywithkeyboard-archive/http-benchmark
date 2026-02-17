## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70980` | `4075` | `81286` |
| **83%** | [Hyper Express](#hyper-express) | `58665` | `3391` | `64535` |
| **32%** | [Node (Default)](#node-default) | `22451` | `5337` | `58113` |
| **31%** | [Fastify](#fastify) | `21946` | `6572` | `35961` |
| **31%** | [Hono](#hono) | `21829` | `6833` | `30498` |
| **25%** | [Koa](#koa) | `17558` | `7828` | `62853` |
| **11%** | [Carbon](#carbon) | `7890` | `1412` | `10130` |
| **9%** | [Express](#express) | `6363` | `1130` | `8254` |


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
    Reqs/sec      8818.04    6432.69   76219.26
    Latency        5.66ms     4.40ms   376.88ms
    HTTP codes:
      1xx - 0, 2xx - 89817, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10183
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10183
    Throughput:     1.80MB/s
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
    Reqs/sec      6308.44    1104.04    8205.33
    Latency        7.92ms     3.57ms   346.08ms
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
    Reqs/sec     21082.20    5473.49   37042.47
    Latency        2.37ms     2.08ms   187.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     20753.73    6295.24   30698.57
    Latency        2.41ms     2.13ms   189.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     58852.09    3316.38   65863.52
    Latency      847.15us    78.15us     2.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.36MB/s
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
    Reqs/sec     18104.97    8045.02   74593.27
    Latency        2.76ms     2.56ms   227.64ms
    HTTP codes:
      1xx - 0, 2xx - 92388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7612
    Throughput:     3.78MB/s
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
    Reqs/sec     23216.90    7010.23   57594.09
    Latency        2.15ms     2.00ms   180.47ms
    HTTP codes:
      1xx - 0, 2xx - 97625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2375
    Throughput:     5.19MB/s
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
    Reqs/sec     71290.03    3265.01   78383.92
    Latency      697.53us   142.25us     9.55ms
    HTTP codes:
      1xx - 0, 2xx - 94979, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5021
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5021
    Throughput:    10.72MB/s
  ```


