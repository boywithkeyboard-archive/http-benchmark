## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73607` | `4148` | `94312` |
| **83%** | [Hyper Express](#hyper-express) | `61097` | `4348` | `70161` |
| **31%** | [Node (Default)](#node-default) | `22924` | `7640` | `74891` |
| **29%** | [Fastify](#fastify) | `21492` | `6030` | `37154` |
| **29%** | [Hono](#hono) | `21440` | `6202` | `30264` |
| **25%** | [Koa](#koa) | `18407` | `8063` | `65579` |
| **10%** | [Carbon](#carbon) | `7417` | `1108` | `10226` |
| **9%** | [Express](#express) | `6397` | `1100` | `8345` |


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
    Reqs/sec      8329.09    4790.94   63345.42
    Latency        6.00ms     4.36ms   376.27ms
    HTTP codes:
      1xx - 0, 2xx - 93552, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6448
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6448
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
    Reqs/sec      6347.54    1097.49    8277.28
    Latency        7.87ms     3.94ms   377.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     22501.83    6858.91   36701.77
    Latency        2.22ms     2.11ms   188.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.10MB/s
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
    Reqs/sec     21939.67    5779.97   30991.76
    Latency        2.28ms     2.14ms   191.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     62654.17    4269.85   79548.33
    Latency      798.69us    82.87us     3.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18655.45    8743.49   64141.12
    Latency        2.67ms     2.57ms   222.44ms
    HTTP codes:
      1xx - 0, 2xx - 92400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7600
    Throughput:     3.91MB/s
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
    Reqs/sec     24093.88    6835.64   65086.38
    Latency        2.07ms     1.78ms   152.53ms
    HTTP codes:
      1xx - 0, 2xx - 97057, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2943
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2943
    Throughput:     5.35MB/s
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
    Reqs/sec     74080.63    3868.28   88197.80
    Latency      671.41us   190.11us     8.89ms
    HTTP codes:
      1xx - 0, 2xx - 95642, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4358
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4358
    Throughput:    11.22MB/s
  ```


