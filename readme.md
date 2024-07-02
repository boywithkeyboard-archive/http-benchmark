## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75554` | `7472` | `85643` |
| **85%** | [Hyper Express](#hyper-express) | `63958` | `4077` | `71644` |
| **33%** | [Node (Default)](#node-default) | `24787` | `7176` | `50811` |
| **32%** | [Fastify](#fastify) | `24093` | `7447` | `37389` |
| **26%** | [Hono](#hono) | `19570` | `5335` | `29331` |
| **24%** | [Koa](#koa) | `17807` | `6724` | `54783` |
| **10%** | [Carbon](#carbon) | `7903` | `1387` | `12039` |
| **8%** | [Express](#express) | `6194` | `961` | `8404` |


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
    Reqs/sec      8410.61    4491.97   49157.97
    Latency        5.91ms     4.37ms   381.46ms
    HTTP codes:
      1xx - 0, 2xx - 92128, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7872
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7872
    Throughput:     1.77MB/s
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
    Reqs/sec      6315.95    1037.51    8507.33
    Latency        7.91ms     3.65ms   351.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23372.66    7379.30   36519.98
    Latency        2.14ms     2.08ms   186.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     21343.03    5834.39   29215.11
    Latency        2.34ms     1.94ms   176.18ms
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
    Reqs/sec     64237.07    3943.02   69723.56
    Latency      776.23us    68.17us     4.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.12MB/s
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
    Reqs/sec     17537.30    6484.21   59201.68
    Latency        2.84ms     2.41ms   212.16ms
    HTTP codes:
      1xx - 0, 2xx - 93844, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6156
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6156
    Throughput:     3.72MB/s
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
    Reqs/sec     24564.18    7520.23   56092.47
    Latency        2.03ms     2.07ms   177.29ms
    HTTP codes:
      1xx - 0, 2xx - 97124, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2876
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2876
    Throughput:     5.47MB/s
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
    Reqs/sec     75003.04    6242.55   77989.20
    Latency      664.62us   225.12us    12.64ms
    HTTP codes:
      1xx - 0, 2xx - 97482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2518
    Throughput:    11.57MB/s
  ```


