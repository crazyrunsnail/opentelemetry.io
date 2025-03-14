---
title: Getting Started
weight: 10
# prettier-ignore
cSpell:ignore: Bazel DBUILD devel DWITH helloworld libcurl openssl tracestate xcode
---

Welcome to the OpenTelemetry C++ getting started guide! This guide will walk you
through the basic steps in installing, instrumenting with, configuring, and
exporting data from OpenTelemetry.

You can use [CMake](https://cmake.org/) or [Bazel](https://bazel.build/) for
building OpenTelemetry C++. The following getting started guide will make use of
CMake and only provide you the most essential steps to have a working example
application (a HTTP server & HTTP client). For more details read
[these instructions](https://github.com/open-telemetry/opentelemetry-cpp/blob/main/INSTALL.md).

## Prerequisites

You can build OpenTelemetry C++ on Windows, macOS or Linux. First you need to
install some dependencies:

{{< tabpane text=true >}} {{% tab "Linux (apt)" %}}

```sh
sudo apt-get install git cmake g++ libcurl4-openssl-dev
```

{{% /tab %}} {{% tab "Linux (yum)" %}}

```sh
sudo yum install git cmake g++ libcurl-devel
```

{{% /tab %}} {{% tab "Linux (alpine)" %}}

```sh
sudo apk add git cmake g++ make curl-dev
```

{{% /tab %}} {{% tab "MacOS (homebrew)" %}}

```sh
xcode-select —install
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git cmake
```

{{% /tab %}} {{< /tabpane >}}

## Building

Get the `opentelemetry-cpp` source:

```shell
git clone --recursive https://github.com/open-telemetry/opentelemetry-cpp
```

Navigate to the repository cloned above, and create the CMake build
configuration:

```shell
cd opentelemetry-cpp
mkdir build && cd build
cmake -DBUILD_TESTING=OFF -DWITH_EXAMPLES_HTTP=ON ..
```

Once build configuration is created, build the CMake targets `http_client` and
`http_server`:

```shell
cmake --build . --target http_client http_server
```

If all goes well, you should find binaries `http_server` and `http_client` in
`./examples/http`:

```console
$ ls ./examples/http
CMakeFiles  Makefile  cmake_install.cmake  http_client  http_server
```

## Run Application

Open two terminals, in the first terminal, start the HTTP server:

```console
$ ./examples/http/http_server
Server is running..Press ctrl-c to exit..
```

In the other terminal, run the HTTP client:

```sh
./examples/http/http_client
```

You should see client output similar to this:

```properties
{
  name          : /helloworld
  trace_id      : 05eec7a55d3544434265dad89d7fe96f
  span_id       : 45fb62c58c907f05
  tracestate    :
  parent_span_id: 0000000000000000
  start         : 1665577080650384378
  duration      : 1640298
  description   :
  span kind     : Client
  status        : Unset
  attributes    :
    http.header.Date: Wed, 12 Oct 2022 12:18:00 GMT
    http.header.Content-Length: 0
    http.status_code: 200
    http.method: GET
    .header.Host: localhost
    http.header.Content-Type: text/plain
    http.header.Connection: keep-alive
    .scheme: http
    http.url: http://localhost:8800/helloworld
  events        :
  links         :
  resources     :
    service.name: unknown_service
    telemetry.sdk.version: 1.6.1
    telemetry.sdk.name: opentelemetry
    telemetry.sdk.language: cpp
  instr-lib     : http-client
}
```

Also the server should dump you a trace to the console:

```properties
{
  name          : /helloworld
  trace_id      : 05eec7a55d3544434265dad89d7fe96f
  span_id       : 8df967d8547813fe
  tracestate    :
  parent_span_id: 45fb62c58c907f05
  start         : 1665577080651459495
  duration      : 46331
  description   :
  span kind     : Server
  status        : Unset
  attributes    :
    http.header.Traceparent: 00-05eec7a55d3544434265dad89d7fe96f-45fb62c58c907f05-01
    http.header.Accept: */*
    http.request_content_length: 0
    http.header.Host: localhost:8800
    http.scheme: http
    http.client_ip: 127.0.0.1:49466
    http.method: GET
    net.host.port: 8800
    net.host.name: localhost
  events        :
    {
      name          : Processing request
      timestamp     : 1665577080651472827
      attributes    :
    }
  links         :
  resources     :
    service.name: unknown_service
    telemetry.sdk.version: 1.6.1
    telemetry.sdk.name: opentelemetry
    telemetry.sdk.language: cpp
  instr-lib     : http-server
}
```

## What's next

Enrich your instrumentation generated automatically with
[manual](/docs/languages/cpp/instrumentation) of your own codebase. This gets
you customized observability data.

You'll also want to configure an appropriate exporter to
[export your telemetry data](/docs/languages/cpp/exporters) to one or more
telemetry backends.

If you'd like to explore a more complex example, take a look at the
[OpenTelemetry Demo](/docs/demo/), which includes the C++ based
[Currency Service](/docs/demo/services/currency/).
