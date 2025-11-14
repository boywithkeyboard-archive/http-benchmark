## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72642` | `3016` | `77730` |
| **84%** | [Hyper Express](#hyper-express) | `61308` | `4716` | `65297` |
| **33%** | [Node (Default)](#node-default) | `24235` | `8117` | `73445` |
| **31%** | [Fastify](#fastify) | `22629` | `7154` | `35342` |
| **28%** | [Hono](#hono) | `20261` | `5732` | `29053` |
| **24%** | [Koa](#koa) | `17751` | `8211` | `64870` |
| **11%** | [Carbon](#carbon) | `7875` | `1427` | `10208` |
| **8%** | [Express](#express) | `6049` | `1030` | `9055` |


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
    Reqs/sec      8331.72    5261.83   69681.68
    Latency        5.99ms     4.50ms   387.81ms
    HTTP codes:
      1xx - 0, 2xx - 92978, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7022
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7022
    Throughput:     1.76MB/s
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
    Reqs/sec      5976.41    1001.20    9522.32
    Latency        8.37ms     3.94ms   375.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     22528.29    7195.01   33747.12
    Latency        2.22ms     2.22ms   197.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.11MB/s
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
    Reqs/sec     19939.01    6040.37   29583.02
    Latency        2.51ms     2.23ms   199.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.50MB/s
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
    Reqs/sec     61906.94    3397.06   66153.44
    Latency      805.36us   105.62us     4.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     19114.54    9430.45   80422.66
    Latency        2.61ms     2.53ms   220.80ms
    HTTP codes:
      1xx - 0, 2xx - 90335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9665
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
    Reqs/sec     24753.61    7879.34   66371.95
    Latency        2.02ms     1.83ms   160.27ms
    HTTP codes:
      1xx - 0, 2xx - 97033, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2967
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2967
    Throughput:     5.50MB/s
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
    Reqs/sec     74411.63    2822.46   82950.05
    Latency      670.00us   133.43us     7.46ms
    HTTP codes:
      1xx - 0, 2xx - 97418, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2582
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2582
    Throughput:    11.46MB/s
  ```


