## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74276` | `1813` | `81180` |
| **84%** | [Hyper Express](#hyper-express) | `62424` | `4718` | `77799` |
| **34%** | [Node (Default)](#node-default) | `25610` | `8720` | `78203` |
| **31%** | [Fastify](#fastify) | `23098` | `7072` | `37360` |
| **28%** | [Hono](#hono) | `21023` | `6316` | `29671` |
| **25%** | [Koa](#koa) | `18521` | `8796` | `80043` |
| **11%** | [Carbon](#carbon) | `7965` | `1385` | `10243` |
| **9%** | [Express](#express) | `6369` | `1095` | `8355` |


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
    Reqs/sec      8616.90    5096.41   69141.92
    Latency        5.79ms     4.29ms   370.11ms
    HTTP codes:
      1xx - 0, 2xx - 93356, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6644
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6644
    Throughput:     1.83MB/s
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
    Reqs/sec      6501.36    1112.78    8317.06
    Latency        7.69ms     3.71ms   354.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     24014.39    7657.63   36328.89
    Latency        2.08ms     2.04ms   183.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20282.09    6033.11   29377.98
    Latency        2.46ms     2.16ms   191.64ms
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
    Reqs/sec     60770.89    3357.66   63210.70
    Latency      820.68us    78.85us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.63MB/s
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
    Reqs/sec     19143.97    8946.08   80102.46
    Latency        2.60ms     2.47ms   214.52ms
    HTTP codes:
      1xx - 0, 2xx - 91936, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8064
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8064
    Throughput:     3.98MB/s
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
    Reqs/sec     25443.39    8456.47   75240.47
    Latency        1.96ms     2.04ms   170.55ms
    HTTP codes:
      1xx - 0, 2xx - 96668, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3332
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3332
    Throughput:     5.63MB/s
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
    Reqs/sec     74533.78    2359.72   80900.11
    Latency      668.98us   144.10us     6.73ms
    HTTP codes:
      1xx - 0, 2xx - 96930, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3070
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3070
    Throughput:    11.43MB/s
  ```


