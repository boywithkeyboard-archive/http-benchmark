## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75026` | `2886` | `82524` |
| **84%** | [Hyper Express](#hyper-express) | `63021` | `4472` | `67314` |
| **32%** | [Node (Default)](#node-default) | `24228` | `6359` | `59404` |
| **30%** | [Fastify](#fastify) | `22798` | `6333` | `35899` |
| **28%** | [Hono](#hono) | `20846` | `5808` | `31315` |
| **26%** | [Koa](#koa) | `19627` | `9389` | `80910` |
| **11%** | [Carbon](#carbon) | `8218` | `1448` | `10444` |
| **9%** | [Express](#express) | `6396` | `1026` | `8417` |


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
    Reqs/sec      8968.73    7038.26   83302.38
    Latency        5.56ms     4.24ms   366.68ms
    HTTP codes:
      1xx - 0, 2xx - 89256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10744
    Throughput:     1.82MB/s
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
    Reqs/sec      6899.75    5280.96   65713.34
    Latency        7.23ms     3.97ms   364.84ms
    HTTP codes:
      1xx - 0, 2xx - 91426, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8574
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8574
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
    Reqs/sec     23784.19    6514.33   37211.31
    Latency        2.10ms     1.91ms   172.62ms
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
    Reqs/sec     20296.36    5072.71   29701.79
    Latency        2.46ms     2.05ms   184.79ms
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
    Reqs/sec     62660.09    3475.42   65951.83
    Latency      795.77us    63.98us     2.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18843.74    7966.92   66751.51
    Latency        2.65ms     2.49ms   215.77ms
    HTTP codes:
      1xx - 0, 2xx - 93214, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6786
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6786
    Throughput:     3.97MB/s
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
    Reqs/sec     24990.25    8373.11   84181.53
    Latency        2.00ms     1.91ms   163.67ms
    HTTP codes:
      1xx - 0, 2xx - 96212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3788
    Throughput:     5.49MB/s
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
    Reqs/sec     75409.37    2878.49   84171.85
    Latency      660.27us   178.93us    10.23ms
    HTTP codes:
      1xx - 0, 2xx - 96660, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3340
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3340
    Throughput:    11.53MB/s
  ```


