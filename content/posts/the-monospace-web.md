---
title: The Monospace Web
description: Showcase for monolog theme for hugo
author: Shankar Murralitharan
email: hello@shankar.blog
date: 2025-01-10T16:43:20+05:30
categories:
    - webdesign
tags:
    - monospace
    - blogging
summary: >
    This post showcases the styling ability of the theme I developed for hugo. This post will be regularly updated as I develop more feature to the monolog theme.
featured: false
draft: false
---

## Introduction

I came across an interesting post in hacker news that piqued my attention, titled "The Monospace web". It is a self explanatory blog post where the author Oskar Wickström wrote about how he created a webpage with monospace font and aligned to a grid.

It was a beautiful piece of work and I wanted to create my own blog using the design. I have only used hugo and jekyll SSG and since hugo was the popular of the two I decided to go with it.

I created this [Monolog theme](https://github.com/shankar/monolog) for Hugo SSG. I named it **monolog** since its meaning rang true to the inteded use for the theme in my personal blog where I rant about whatever I want. Also the word has "mono" in it, as in monospace.

So in this post I am gonna showcase the styling ability of the theme.
Check out the source code for this blog at [https://github.com/shankar/shankar](https://github.com/shankar/shankar)

## The Basics

The post is made entirely of markdown with shortcodes for elements or styling not supported by markdown

Look at this horizontal break using shortcode :

{{< hr >}}

Lovely. We can hide stuff in the `<details>` element.

{{< details summary="A short summary of contents" >}}
This is the detail behind the summary.
{{< /details >}}

This is a plain old bulleted list:

* Banana
* Paper boat
* Cucumber
* Rocket

Ordered lists look pretty much as you'd expect:

1. Goals
1. Motivations
    1. Intrinsic
    1. Extrinsic
1. Second-order effects

It's nice to visualize trees.
This is a regular unordered list with a `tree` class:

{{< tree root="/dev/nvme0n1p2" >}}
* usr
    * local
    * share
    * libexec
    * include
    * sbin
    * src
    * lib64
    * lib
    * bin
    * games
        * solitaire
        * snake
        * tic-tac-toe
    * media
* media
* run
* tmp
{{< /tree >}}

## Code block

Code blocks are automatically highlighted thanks to hugo using chroma. We just have to provide the language used.

```zig
const std = @import("std");

pub fn main() void {
    std.debug.print("hello world !!\n", .{});
}
```

or if we require filename for the the codeblock we can use `file` shortcode like this:
{{< file "hello.c" >}}
```c
#include <stdio.h>
int main() {
   // printf() displays the string inside quotation
   printf("Hello, World!");
   return 0;
}
```

{{< /file >}}

## Terminal

If we need to show some commandline output we can make use of the `output` shortcode.

{{< output >}}
<prompt><kbd>brew update</kbd>
==> Updating Homebrew...
Already up-to-date.
<prompt><kbd>su -</kbd>
<rootprompt><cursor>
{{< /output >}}


## ASCII Drawings

We can draw in `<pre>` tags using [box-drawing characters](https://en.wikipedia.org/wiki/Box-drawing_characters):

```
╭─────────────────╮
│ MONOSPACE ROCKS │
╰─────────────────╯
```

To have it stand out a bit more, we can wrap it in a `<figure>` tag, and why not also add a `<figcaption>`.

{{< prefmt caption="Example: Message passing." >}}
┌───────┐ ┌───────┐ ┌───────┐
│Actor 1│ │Actor 2│ │Actor 3│
└───┬───┘ └───┬───┘ └───┬───┘
    │         │         │
    │         │  msg 1  │
    │         │────────►│
    │         │         │
    │  msg 2  │         │
    │────────►│         │
┌───┴───┐ ┌───┴───┐ ┌───┴───┐
│Actor 1│ │Actor 2│ │Actor 3│
└───────┘ └───────┘ └───────┘
{{< /prefmt >}}

Let's go wild and draw a chart!

{{< prefmt >}}
                      Things I Have

    │                                     ████ Usable
15  │
    │                                     ░░░░ Broken
    │
12  │             ░
    │             ░
    │   ░         ░
 9  │   ░         ░
    │   ░         ░
    │   ░         ░                    ░
 6  │   █         ░         ░          ░
    │   █         ░         ░          ░
    │   █         ░         █          ░
 3  │   █         █         █          ░
    │   █         █         █          ░
    │   █         █         █          ░
 0  └───▀─────────▀─────────▀──────────▀─────────────
      Socks     Jeans     Shirts   USB Drives
{{< /prefmt >}}

# Media

Media objects are supported, like images and video:

{{< figure type="img"
    src="https://images.ctfassets.net/g8qtv9gzg47d/4y9eVGaMmQSiSwiyyYQUyq/c2c36a483681fdf356fac4a2ab4e8d38/hilary-rhoda-28.jpg?fl=progressive&fm=jpg&q=80"
    alt="Hilary Rhoda"
    title="Hey its my favourite model Hilary Rhoda!!"
>}}

{{< figure type="video"
    src="https://upload.wikimedia.org/wikipedia/commons/e/e0/The_Center_of_the_Web_%281914%29.webm"
    title="The Center of the Web (1914), Wikimedia"
    link="https://en.wikisource.org/wiki/Page:The_Center_of_the_Web_(1914).webm/11"
>}}

## Discussion

If you are wondering what color scheme I am using, its catppuccin latte for light scheme and catppuccin mocha for dark scheme
That is all for now folks. As I work futher on the theme ill update this post to showcase more elements.

I thank [Oskar Wickström](https://wickstrom.tech) for kindling my interest in monospace web.
