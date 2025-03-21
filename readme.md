## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75037` | `6107` | `87229` |
| **85%** | [Hyper Express](#hyper-express) | `64069` | `4638` | `82080` |
| **36%** | [Node (Default)](#node-default) | `27266` | `9162` | `56142` |
| **33%** | [Fastify](#fastify) | `24955` | `8068` | `35840` |
| **29%** | [Hono](#hono) | `21387` | `5997` | `30276` |
| **25%** | [Koa](#koa) | `18785` | `6770` | `55994` |
| **11%** | [Carbon](#carbon) | `8179` | `1426` | `10138` |
| **9%** | [Express](#express) | `6413` | `1027` | `8388` |


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
    Reqs/sec      9000.88    5388.75   55673.49
    Latency        5.55ms     4.33ms   371.89ms
    HTTP codes:
      1xx - 0, 2xx - 91102, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8898
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8898
    Throughput:     1.86MB/s
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
    Reqs/sec      6758.14    4186.49   48758.87
    Latency        7.39ms     4.07ms   369.85ms
    HTTP codes:
      1xx - 0, 2xx - 92650, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7350
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7350
    Throughput:     1.79MB/s
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
    Reqs/sec     24653.02    7872.24   36370.64
    Latency        2.03ms     2.03ms   179.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     20912.24    5852.72   29003.56
    Latency        2.39ms     2.01ms   181.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     64154.54    3465.35   71015.46
    Latency      777.16us    66.56us     3.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     18324.40    7225.97   54343.87
    Latency        2.72ms     2.47ms   216.93ms
    HTTP codes:
      1xx - 0, 2xx - 92087, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7913
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7913
    Throughput:     3.82MB/s
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
    Reqs/sec     24868.36    7119.43   46971.92
    Latency        2.01ms     1.89ms   166.04ms
    HTTP codes:
      1xx - 0, 2xx - 98019, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1981
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1981
    Throughput:     5.58MB/s
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
    Reqs/sec     75005.67    5708.85   92024.75
    Latency      663.38us   205.31us     8.97ms
    HTTP codes:
      1xx - 0, 2xx - 96678, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3322
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3322
    Throughput:    11.47MB/s
  ```


