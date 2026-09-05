# C++ HTTP Web Server and Closed-Loop Load Generator

A small systems-programming project that pairs a multithreaded HTTP/1.1 file
server with a closed-loop workload generator. It was built for IIT Bombay's
CS 744 course to explore sockets, worker pools, bounded request queues,
throughput, and response-time measurement without a web framework.

> **Historical project:** this repository is archived and is not maintained.
> It is educational code, not a production HTTP server or benchmarking suite.

## What is implemented

- IPv4 TCP server using POSIX sockets
- Four-thread worker pool and a bounded circular connection queue
- Basic `GET` request parsing and static files under `html_files/`
- One load-generator thread per simulated user
- Configurable concurrency, think time, and test duration
- Aggregate request count, throughput, and average round-trip time

The implementation intentionally covers only a narrow subset of HTTP. It does
not provide TLS, robust request validation, access controls, or production
hardening.

## Build

Requirements: a Unix-like system, GNU Make, and a C++ compiler with POSIX
threads.

```bash
git clone https://github.com/xzaviourr/cpp-web-server-load-generator.git
cd cpp-web-server-load-generator
make all
```

This creates `server` and `generator`.

## Run

Start the server (the implementation accepts a port, despite older
documentation referring to a hostname):

```bash
./server 8080
```

In another terminal, run a 30-second local test with 10 users and a 0.1-second
think time:

```bash
./generator 127.0.0.1 8080 10 0.1 30
```

Use the generator only against systems you own or are explicitly authorized to
test. Generated traffic can consume significant resources.

## Recorded experiment

`load-gen-output.csv` contains the captured measurements and
`load-gen-output.png` visualizes them. The accompanying
`Load Testing of Web Server.pdf` documents the course experiment and analysis.
These are historical measurements from one environment, not portable
performance guarantees.

![Recorded load-generator results](./load-gen-output.png)

## Limitations

- Linux/POSIX APIs and GNU-oriented headers are used.
- The server supports basic GET traffic only.
- Error handling and HTTP compliance are intentionally minimal.
- The generator reports aggregate client-side timing and is not a calibrated
  benchmarking tool.

## License

The original source code is available under the [MIT License](LICENSE).
Course material and generated experiment artifacts remain subject to their
respective terms.
