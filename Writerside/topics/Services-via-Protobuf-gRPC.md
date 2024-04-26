# Services via Protobuf &amp; gRPC

To offer AI services across different programming languages, the `org.aiddl.external.grpc` set of 
libraries offer a variety of abstractions for:

- Acting and sensing
- Sending and receiving data
- Single functions
- Remote containers

All of these are implemented in a way that hides the fact that requests are handled over
gRPC instead of locally. 
