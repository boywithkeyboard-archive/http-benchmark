## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73229` | `4659` | `89705` |
| **81%** | [Hyper Express](#hyper-express) | `59230` | `3696` | `66723` |
| **29%** | [Hono](#hono) | `21571` | `6760` | `31204` |
| **28%** | [Fastify](#fastify) | `20626` | `5302` | `37595` |
| **28%** | [Node (Default)](#node-default) | `20391` | `4737` | `56630` |
| **24%** | [Koa](#koa) | `17890` | `8853` | `69165` |
| **10%** | [Carbon](#carbon) | `7348` | `1169` | `10252` |
| **8%** | [Express](#express) | `6175` | `1053` | `8325` |


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
    Reqs/sec      7791.88    5237.09   66842.98
    Latency        6.40ms     4.45ms   383.35ms
    HTTP codes:
      1xx - 0, 2xx - 92117, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7883
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7883
    Throughput:     1.63MB/s
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
    Reqs/sec      6132.26    1042.70    8280.25
    Latency        8.15ms     3.92ms   373.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     21241.39    5620.32   37370.48
    Latency        2.35ms     1.98ms   181.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     22545.44    7231.71   31296.81
    Latency        2.22ms     2.01ms   182.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.09MB/s
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
    Reqs/sec     60393.32    3771.60   67118.17
    Latency      825.62us    91.69us     3.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.58MB/s
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
    Reqs/sec     17642.06    8027.29   66287.14
    Latency        2.83ms     2.50ms   215.47ms
    HTTP codes:
      1xx - 0, 2xx - 92839, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7161
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7161
    Throughput:     3.71MB/s
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
    Reqs/sec     20405.47    4701.17   53750.66
    Latency        2.45ms     1.88ms   162.94ms
    HTTP codes:
      1xx - 0, 2xx - 97528, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2472
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2472
    Throughput:     4.56MB/s
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
    Reqs/sec     73586.25    6181.14   96649.22
    Latency      676.57us   177.32us     9.09ms
    HTTP codes:
      1xx - 0, 2xx - 96655, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3345
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3345
    Throughput:    11.26MB/s
  ```


