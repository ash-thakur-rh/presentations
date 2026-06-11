---
title: 'Building CLI Tools in Go'
date: 2026-06-11
description: 'A talk about building great CLI tools with Go'
event: 'GopherCon 2026'
tags: ['go', 'cli', 'tools']
theme: black
fonts:
  - 'JetBrains Mono:wght@400;700'
  - 'Space Grotesk:wght@400;700'
---

<section>

## Building CLI Tools in Go

<p class="subtitle">Ashish Thakur · GopherCon 2026</p>

</section>

<section>

## Why Go for CLIs?

- Single binary, zero dependencies
- Fast startup time
- Cross-compilation built in
- Rich standard library

</section>

<section>

## The Basics

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    if len(os.Args) < 2 {
        fmt.Fprintln(os.Stderr, "usage: greet <name>")
        os.Exit(1)
    }
    fmt.Printf("Hello, %s!\n", os.Args[1])
}
```

</section>

<section>

## Thank You

Questions?

</section>
