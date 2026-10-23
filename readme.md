## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `102655` | `4082` | `109956` |
| **88%** | [Hyper Express](#hyper-express) | `89902` | `4259` | `94820` |
| **45%** | [Node (Default)](#node-default) | `46688` | `13981` | `100644` |
| **44%** | [Fastify](#fastify) | `45045` | `11997` | `67393` |
| **38%** | [Hono](#hono) | `39190` | `10860` | `60793` |
| **38%** | [Koa](#koa) | `38737` | `14579` | `92263` |
| **11%** | [Carbon](#carbon) | `11536` | `2541` | `17221` |
| **9%** | [Express](#express) | `9493` | `2015` | `13755` |


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
    Reqs/sec     12902.80    8895.46  111104.13
    Latency        3.87ms     3.49ms   295.64ms
    HTTP codes:
      1xx - 0, 2xx - 91385, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8615
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8615
    Throughput:     2.68MB/s
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
    Reqs/sec     10465.60    9234.51   98738.50
    Latency        4.77ms     2.81ms   258.84ms
    HTTP codes:
      1xx - 0, 2xx - 88363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11637
    Throughput:     2.65MB/s
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
    Reqs/sec     46720.06   13569.34   68605.65
    Latency        1.07ms     1.51ms   134.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.59MB/s
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
    Reqs/sec     38650.98   10258.98   61242.67
    Latency        1.29ms     1.35ms   123.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     90119.23    3844.89   94263.97
    Latency      553.60us    45.45us     2.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.80MB/s
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
    Reqs/sec     38606.90   16684.53   96485.93
    Latency        1.29ms     1.79ms   154.19ms
    HTTP codes:
      1xx - 0, 2xx - 92358, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7642
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7642
    Throughput:     8.08MB/s
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
    Reqs/sec     48381.68   13907.41   99673.18
    Latency        1.03ms     1.26ms   103.17ms
    HTTP codes:
      1xx - 0, 2xx - 94247, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5753
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5753
    Throughput:    10.42MB/s
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
    Reqs/sec    101581.78    3693.47  109083.65
    Latency      490.77us   118.41us     5.35ms
    HTTP codes:
      1xx - 0, 2xx - 96781, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3219
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3219
    Throughput:    15.54MB/s
  ```


