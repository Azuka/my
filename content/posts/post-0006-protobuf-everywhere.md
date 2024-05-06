---
categories: []
date: "2024-03-25T20:47:36-07:00"
draft: true
images: []
tags: ["protobuf", "buf", "connect"]
title: Proto(Buf) everywhere
summary: My interesting journey with using protocol buffers
---

```bash
protoc \
	-I $(PWD) \
	-I $(PWD)/definitions \
	-I $(PWD)/vendor/github.com/grpc-ecosystem/grpc-gateway/v2 \
	--openapiv2_out=json_names_for_fields=false,allow_merge=true,merge_file_name=definitions/api,logtostderr=true,grpc_api_configuration=definitions/rpc.api.yaml:. definitions/rpc*.proto

rm -rfv definitions/*.go
protoc \
	-I $(PWD) \
	-I $(PWD)/definitions \
	-I $(PWD)/vendor/github.com/grpc-ecosystem/grpc-gateway/v2 \
	--grpc-gateway_out=logtostderr=true,grpc_api_configuration=definitions/rpc.api.yaml,paths=source_relative:. definitions/rpc*.proto
protoc \
	-I $(PWD) \
	-I $(PWD)/definitions \
	-I $(PWD)/vendor/github.com/grpc-ecosystem/grpc-gateway/v2 \
	--go_out=. \
	--go-grpc_out=. \
	--go-grpc_opt=paths=source_relative \
	--go_opt=paths=source_relative \
	definitions/type*.proto definitions/rpc*.proto
go run ./tools/discovery > /dev/null
```

I went poking in one of the scripts I wrote for [SingleSend](./post-0003-a-billing-problem/) (now defunct) in 2018 and was greeted with this monstrosity.

My introduction to protocol buffers and the surrounding ecosystem including gRPC started around then. I'd designed a LOT of APIs before, but was dissatisfied with OpenAPI/Swagger and its rather verbose structure, as well as the poor support for version 3 in a lot of the tooling available.

gRPC promised a simpler way to define APIs, perhaps a little too simply, because there's no [418 status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/418), but it took away a lot of the cruft involved in generating apis:

- Generated clients for different programming languages that just work (with some caveats)
- Generated server interfaces and boilerplate
- A secure by default protocol

The generation was rife with issues around how protoc expected proto files to be organized, how to download and use Google's [well-known-types](https://protobuf.dev/reference/protobuf/google.protobuf/), and installing plugins. In the meantime, I discovered I had to have some translation layer on the frontend because [browsers weren't quite ready for http2/h2c](https://grpc.io/blog/state-of-grpc-web/). I settled on [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway) over [grpc-web](https://github.com/improbable-eng/grpc-web) because I had some issues getting the latter to work with Angular.

# Buf

[Buf Cli](https://buf.build/docs/ecosystem/cli-overview) came out a year later and it was honestly refreshing. Going from a lot of protoc arguments to this was so much cleaner:

```yaml
version: v1
plugins:
  - name: go
    out: .
    opt:
      - paths=source_relative
  - name: go-grpc
    out: .
    opt:
      - paths=source_relative
  - name: grpc-gateway
    out: .
    opt:
      - paths=source_relative
      - generate_unbound_methods=true
  - name: openapiv2
    out: .
  - remote: buf.build/demolab/plugins/twirp:v8.1.2-1
    out: .
    opt:
      - paths=source_relative
  - remote: buf.build/timostamm/plugins/protobuf-ts:v2.8.0-1
    out: ./libs/proto/src/lib/definitions
    opt:
      - long_type_number
      - enable_angular_annotations
      - force_server_none
      - eslint_disable
      - ts_nocheck
```

A couple projects made that possible:

- [Twirp](https://github.com/twitchtv/twirp): Pretty much the same protobufs, but a lighter rpc framework without the cruft of gRPC. I didn't have any streaming use-cases, so this was perfect.
- [protobuf-ts](https://github.com/timostamm/protobuf-ts): Started by Timo Stamm who works for Buf (as of the date of this post). This got better typing and support on the frontend (specifically for Angular) without having to first use openapi annotations for grpc-gateway, then generating a client based on the openapi file.

For a while I wrote Twirp APIs and used protobuf-ts on the frontend, and that served my needs until the middle of 2022.

# Enter Connect

In June 2022, an article caught my eye. Buf [released Connect](https://buf.build/blog/connect-a-better-grpc), and things had to change for me again. Everything in there (and more) were issues I had with gRPC, now solved in an elegant way.

gRPC, JSON and grpc-web served from one server using the same underlying code, and intelligently routed based on the content-type header? Generated clients to work with the latter two, with a fallback on normal gRPC clients for languages without official Connect support? I was in love.
