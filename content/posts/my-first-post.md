---
title: "My First Post"
date: 2021-01-23T19:18:22+01:00
draft: true
---

# Hello World

This is my demo post where I check some things about the Theme I'm using.

This example page will contain some rust as well as some Markdown and
some other stuff.
There will be a bit of useless text on the start for some reasons.
Read more on my [GitHub] page.

Here we have a nice table:

| Foo | Bar |
| --- | --- |
| 123 | 456 |

```rust
extern crate tokio;
extern crate anyhow;

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    println!("Hello, World!");
    
    Ok(())
}
```

[GitHub]: https://github.com/rappet