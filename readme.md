## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73858` | `3873` | `81799` |
| **84%** | [Hyper Express](#hyper-express) | `62147` | `3612` | `68981` |
| **33%** | [Node (Default)](#node-default) | `24208` | `7249` | `62241` |
| **31%** | [Fastify](#fastify) | `22561` | `6661` | `36158` |
| **28%** | [Hono](#hono) | `20993` | `5947` | `31484` |
| **25%** | [Koa](#koa) | `18280` | `8337` | `68676` |
| **11%** | [Carbon](#carbon) | `8262` | `1464` | `10521` |
| **9%** | [Express](#express) | `6303` | `1064` | `8337` |


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
    Reqs/sec      8743.93    5822.97   67313.95
    Latency        5.70ms     4.29ms   374.18ms
    HTTP codes:
      1xx - 0, 2xx - 91092, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8908
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8908
    Throughput:     1.81MB/s
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
    Reqs/sec      6423.76    1098.96    8397.07
    Latency        7.78ms     3.73ms   353.57ms
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
    Reqs/sec     23819.52    7507.01   35560.54
    Latency        2.10ms     2.12ms   190.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     21114.20    5842.34   29970.76
    Latency        2.37ms     2.11ms   187.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61907.81    3739.63   69048.40
    Latency      805.21us    77.29us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18047.70    7538.73   68007.67
    Latency        2.76ms     2.47ms   220.69ms
    HTTP codes:
      1xx - 0, 2xx - 93112, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6888
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6888
    Throughput:     3.80MB/s
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
    Reqs/sec     24519.37    7213.13   72325.61
    Latency        2.03ms     2.00ms   171.21ms
    HTTP codes:
      1xx - 0, 2xx - 96487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3513
    Throughput:     5.42MB/s
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
    Reqs/sec     74168.81    3338.07   85714.86
    Latency      671.78us   253.42us    14.70ms
    HTTP codes:
      1xx - 0, 2xx - 96534, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3466
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3466
    Throughput:    11.33MB/s
  ```


