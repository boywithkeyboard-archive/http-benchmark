## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75988` | `2592` | `83269` |
| **84%** | [Hyper Express](#hyper-express) | `64130` | `3382` | `68019` |
| **35%** | [Node (Default)](#node-default) | `26556` | `8823` | `73907` |
| **28%** | [Fastify](#fastify) | `21076` | `4781` | `30203` |
| **25%** | [Koa](#koa) | `19222` | `8802` | `66525` |
| **24%** | [Hono](#hono) | `18332` | `4045` | `28995` |
| **11%** | [Carbon](#carbon) | `8321` | `1431` | `10365` |
| **8%** | [Express](#express) | `6278` | `976` | `8374` |


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
    Reqs/sec      9276.47    7884.50   80219.74
    Latency        5.38ms     4.33ms   373.33ms
    HTTP codes:
      1xx - 0, 2xx - 87803, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12197
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12197
    Throughput:     1.85MB/s
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
    Reqs/sec      7240.89    6706.91   80421.68
    Latency        6.89ms     3.98ms   361.84ms
    HTTP codes:
      1xx - 0, 2xx - 88430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11570
    Throughput:     1.83MB/s
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
    Reqs/sec     24982.50    7667.18   35891.28
    Latency        2.00ms     1.97ms   180.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.66MB/s
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
    Reqs/sec     20962.28    6087.61   29980.29
    Latency        2.38ms     1.93ms   176.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63371.20    3558.95   65940.23
    Latency      786.77us    66.85us     2.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     19544.19    9098.19   79191.29
    Latency        2.55ms     2.41ms   211.74ms
    HTTP codes:
      1xx - 0, 2xx - 91565, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8435
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8435
    Throughput:     4.05MB/s
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
    Reqs/sec     26049.42    8321.13   81644.54
    Latency        1.92ms     2.02ms   166.38ms
    HTTP codes:
      1xx - 0, 2xx - 96506, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3494
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3494
    Throughput:     5.76MB/s
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
    Reqs/sec     75807.81    2403.75   83729.16
    Latency      656.32us   169.79us    13.40ms
    HTTP codes:
      1xx - 0, 2xx - 96145, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3855
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3855
    Throughput:    11.53MB/s
  ```


