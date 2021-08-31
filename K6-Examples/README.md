# k6 Performance Tests

Performance tests examples show casing the features of K6.

- [Setup](#setup)
- [Run a test](#run-a-test)
- [Configuration options](#configuration-options)


## Setup

Install k6. MacOS example is shown below:

```shell
$ brew install k6
```


## Run a test

Choose a test from  `test-examples`.


When the test is complete a summary of results will be shown:

```
          /\      |‾‾| /‾‾/   /‾‾/   
     /\  /  \     |  |/  /   /  /    
    /  \/    \    |     (   /   ‾‾\  
   /          \   |  |\  \ |  (‾)  | 
  / __________ \  |__| \__\ \_____/ .io

  execution: local
     script: hello-world.js
     output: -

  scenarios: (100.00%) 1 scenario, 1 max VUs, 10m30s max duration (incl. graceful stop):
           * default: 1 iterations for each of 1 VUs (maxDuration: 10m0s, gracefulStop: 30s)


running (00m01.6s), 0/1 VUs, 1 complete and 0 interrupted iterations
default ✓ [======================================] 1 VUs  00m01.6s/10m0s  1/1 iters, 1 per VU

     data_received..................: 17 kB 10 kB/s
     data_sent......................: 693 B 425 B/s
     http_req_blocked...............: avg=442.19ms min=442.19ms med=442.19ms max=442.19ms p(90)=442.19ms p(95)=442.19ms
     http_req_connecting............: avg=103.33ms min=103.33ms med=103.33ms max=103.33ms p(90)=103.33ms p(95)=103.33ms
     http_req_duration..............: avg=183.54ms min=183.54ms med=183.54ms max=183.54ms p(90)=183.54ms p(95)=183.54ms
       { expected_response:true }...: avg=183.54ms min=183.54ms med=183.54ms max=183.54ms p(90)=183.54ms p(95)=183.54ms
     http_req_failed................: 0.00% ✓ 0   ✗ 1  
     http_req_receiving.............: avg=88.14ms  min=88.14ms  med=88.14ms  max=88.14ms  p(90)=88.14ms  p(95)=88.14ms 
     http_req_sending...............: avg=682µs    min=682µs    med=682µs    max=682µs    p(90)=682µs    p(95)=682µs   
     http_req_tls_handshaking.......: avg=337.26ms min=337.26ms med=337.26ms max=337.26ms p(90)=337.26ms p(95)=337.26ms
     http_req_waiting...............: avg=94.72ms  min=94.72ms  med=94.72ms  max=94.72ms  p(90)=94.72ms  p(95)=94.72ms 
     http_reqs......................: 1     0.613891/s
     iteration_duration.............: avg=1.62s    min=1.62s    med=1.62s    max=1.62s    p(90)=1.62s    p(95)=1.62s   
     iterations.....................: 1     0.613891/s
     vus............................: 1     min=1 max=1
     vus_max........................: 1     min=1 max=1
```

If required, use the `--http-debug="full"` flag to see full HTTP details of
requests and responses. This is useful for debugging test failures.

```shell
$ npm test -- --http-debug="full"
```



