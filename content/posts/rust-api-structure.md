---
title: "Rust Api Structure"
date: 2021-01-23T22:21:22+01:00
categories:
    - rust
tags:
    - rust
    - api
    - warp
    - openapi
    - swagger-ui
---

**Unfinished!**

In this blog post I will describe the state of the rust API ecosystem
and give an introduction how to write testable production ready APIs.

*Since I don't have any content for my blog yet,
I will just post this unfinished post about rust APIs.
It will contain some mistakes.
I will remove this text if the blog post is finished.*

## The state of rust for Web

In my personal opinion, rust for web is ready right now,
as also correctly stated by [arewewebyet].
But this only includes the main building blocks.

With the release of [Tokio 1.0] it has a solid networking stack that will be
stable for the next years to come,
as well as other components such as [Serde].

Libraries like [hyper], which provide the foundation for many
web-frameworks do not have a stable 1.0 version right now.
But with the 1.0 version of Tokio that might not take long.

There are many great web frameworks.
The two most notable ones are [actix-web] and [warp].
actix-web is allready at the 4th major version,
while warp did not reach 1.0 yet.

If you want an easy library to use *right now*,
consider using [actix-web] or [rocket].
I will focus more on warp in this article because I bet
(and I might be wrong here) that it will be the more stable library
in the future and will also integrate better with other crates.

Since all of them use [Tokio], this guide should integrate well with
all three mentioned frameworks.

## Structuring an API crate

This part will mostly talk about the structure of the API crate.
It is always a good idea to separate the business logic
of your crate from the API handling part.
This will also help to replace the actual HTTP library in the future.
If you mostly stick to database drivers and other stuff that depends
on [Tokio] 1.0, that shouldn't be a problem anymore.

You should implement the business logic as a *service*,
while implementing the actual data structures it uses in the *model*.

If you want to get content from a database,
the service is the right place for it.
The actual JSON objects that your API will return
should you define in your model,
if they also fit to your API design.

A service can also be the right place if you want to make calls to an external API.

The structure of your project might look like this:

```
/my-super-api
+- Cargo.toml
+- main.rs
+- todos
   +- mod.rs
   +- model.rs
   +- service.rs
+- api
   +- v1
      +- <here goes your API definition>
```

You can just export the types from the `model` and `service`
from your `todos` module.
Your `todos/mod.rs` might look like this:

```rust
// the modules for the model and service will not be public
mod model;
mod service;

// so we will export the types from them
pub use model::Todo;
pub use service::TodoService;
```

Your service should implement `Clone` and `Send` traits,
so your web framework can distribute it over multiple threads.
Most database connectors and connection pools also implement it,
so you might just need to add a `#[derive(Clone)]` to your type.

```rust
#[derive(Clone)]
pub struct TodoService {
    db: YourDatabaseConnector,
}
```

[arewewebyet]: https://www.arewewebyet.org/
[Tokio]: https://tokio.rs/
[Tokio 1.0]: https://tokio.rs/blog/2020-12-tokio-1-0
[Serde]: https://serde.rs/
[hyper]: https://github.com/hyperium/hyper