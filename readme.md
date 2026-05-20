## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74135` | `3432` | `83034` |
| **84%** | [Hyper Express](#hyper-express) | `62412` | `4152` | `75035` |
| **30%** | [Node (Default)](#node-default) | `22330` | `7808` | `83140` |
| **29%** | [Hono](#hono) | `21644` | `6265` | `30810` |
| **28%** | [Fastify](#fastify) | `21034` | `5313` | `37422` |
| **24%** | [Koa](#koa) | `17430` | `8436` | `68550` |
| **10%** | [Carbon](#carbon) | `7411` | `1152` | `10225` |
| **8%** | [Express](#express) | `6272` | `1029` | `8243` |


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
    Reqs/sec      8135.52    5798.00   70513.15
    Latency        6.13ms     4.46ms   380.77ms
    HTTP codes:
      1xx - 0, 2xx - 90890, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9110
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9110
    Throughput:     1.68MB/s
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
    Reqs/sec      6192.11    1012.24    8198.74
    Latency        8.07ms     3.85ms   363.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     21403.85    5417.18   37113.83
    Latency        2.34ms     2.02ms   182.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     21205.52    6220.48   31277.83
    Latency        2.36ms     2.09ms   188.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     61992.24    3586.72   68686.69
    Latency      804.40us    80.56us     3.36ms
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
    Reqs/sec     17681.48    8949.82   78082.37
    Latency        2.82ms     2.46ms   214.65ms
    HTTP codes:
      1xx - 0, 2xx - 91116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8884
    Throughput:     3.65MB/s
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
    Reqs/sec     21708.61    6489.76   63347.05
    Latency        2.30ms     1.90ms   164.31ms
    HTTP codes:
      1xx - 0, 2xx - 97390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2610
    Throughput:     4.84MB/s
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
    Reqs/sec     74750.97    3582.58   88959.75
    Latency      666.03us   176.25us     9.14ms
    HTTP codes:
      1xx - 0, 2xx - 96441, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3559
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3559
    Throughput:    11.41MB/s
  ```


