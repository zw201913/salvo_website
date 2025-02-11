---
title: "CORS"
weight: 3030
menu:
  book:
    parent: "middlewares"
---

CORS 中間件可以用於 [跨域資源共享](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS).

```rust
use salvo::prelude::*;
use salvo_extra::cors::CorsHandler;

#[handler]
async fn hello() -> &'static str {
    "hello"
}

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();

    let cors_handler = CorsHandler::builder()
        .with_allow_origin("https://salvo.rs")
        .with_allow_methods(vec!["GET", "POST", "DELETE"])
        .build();

    let router = Router::with_hoop(cors_handler).get(hello);
    Server::new(TcpListener::bind("127.0.0.1:7878")).serve(router).await;
}
```