---
title: A Monologue on Zig Import
author: Shankar Murralitharan
email: hello@shankar.blog
date: 2025-01-13T03:28:33+05:30
categories:
    - programming
tags:
    - zig
    - coding
    - programming
    - learning
series:
    - Learning zig
summary: >
    learning how @import function and pub keyword works. @import function is used to import declaration from other file sources. Pub keyword is used to expose declarations from one file to another on import.
featured: false
draft: false
---

## Introduction

I am learning zig and writing here is a way of me documenting what I learned from various sources. I may have made mistakes. Please contact me at [hello@shankar.blog](mailto:hello@shankar.blog) if you find any error. Lets learn together.

I intend to keep this post and subsequent posts on zig updated. If at any point the examples don't work or the concepts get outdated please contact me at the email provided above.

This is not going to be a zig tutorial. I am not going to talk about what zig language is or how to install it or even the syntax. It is going to be concepts of how zig works. So lets dive in.

Here is the obligatory hello world program.

{{< file "hello.zig" >}}
```zig
const std = @import("std");

pub fn main() void {
std.debug.print("Hello, World!!\n"), .{});
}
```
{{< /file >}}

Saving the above file and running the program using the command `zig run hello.zig` will predictably print `Hello, World!!`. In the first line the program is importing code from standard library. In this post we are going to appreciate how `@import` function works from the perspective of a programmer.

To understand how importing works we need to first understand namespaces.

## Namespace

A namespace is a container for related declarations. Anything within two curly braces is a namespace. The only exception to this rule is zig files.

## @import function

@import is a builtin funtion.

When we need access to the declarations in another file we import that file using the `@import` function. `@import` function takes a filepath or package name string literal as argument and returns a `struct type`. Here the **file path cannot reach above the directory of the root file**. Root file is the entry point file to the program or library.

Providing a constant as argument for @import function will throw an error. It can accept only string literal.

```zig
const standard = "std";
const std = @import(standard);
...
```

If we make the above changes to the _hello.zig_ file. The compiler will not compile and throw `error: @import operand must be a string literal`.

There are 3 packages that are always available:
* std
* builtin
* root

Explaining these packages is beyond the scope of this post.

Remember @import function returns a struct ? let us write some code to demonstrate that.
{{< file "hello.zig" >}}
```zig
const print = @import("std").debug.print;
const Student = @import("student.zig");

pub fn main() void {
    print("Hello {s}!!\n", .{Student.fullname});
    var kid: Student = .{.roll_number = 32};
    print("Roll number: {d}\n", .{kid.getRoll()});
    print("Today your age is {d}\n", .{Student.getAge()});
    print("Happy Birthday!!\n", .{});
    Student.addAge();
    print("Now your age is {d}\n", .{Student.getAge()});
}
```
{{< /file >}}

{{< file "student.zig" >}}
```zig
// These two are container level variables
pub const fullname = "John Doe";
//Cannot be accessed by any other file directly
var age: u8 = 18;

// This is a struct field
roll_number: u8,

pub fn getAge() u8 {
    return age;
}

pub fn addAge() {
    age += 1;
}

pub fn getRoll(student: @This()) u8 {
    return student.roll_number;
}
```
{{< /file >}}

@import supports circular imports. Again lets write some code to demonstrate this.

{{< file "main.zig" >}}
```zig
const one = @import("one.zig");
pub fn main() void {
    one.func();
}
```
{{< /file >}}

{{< file "one.zig" >}}
```zig
const std = @import("std");
const two = @import("two.zig");

pub fn func() void {
    std.debug.print("This is one.zig file\n", .{});
    two.func();
}
```
{{< /file >}}

{{< file "two.zig" >}}
```zig
const std = @import("std");
const one = @import("one.zig");

var counter: u8 = 1;

pub fn func() void {
    std.debug.print("Count: {d}\n", .{counter});
    if (counter == 10) return;
    counter += 1;
    one.func();
}
```
{{< /file >}}

If we run the program with `zig run main.zig`, every 2 seconds it will print the below output with increasing count value by 1 till it reaches 10

{{< output >}}
This is one.zig file
Count: 1
{{< /output >}}

To demonstrate circular import the `one.zig` file imports and runs `func()` from `two.zig` file and `two.zig` file imports and runs `func()` from `one.zig`.

## Pub keyword

You would have perceptively noticed the keyword `pub` in the code. The `pub` keyword decides which declarations in a file are exposed when that file is imported by another file. Enough talk lets code

{{< file "expose.zig" >}}
```zig
pub const shankar = "Shankar Murralitharan";

pub const MyPublicStruct = struct {
    pub const nested_public = "Public const in MyPublicStruct";
};

const MyPrivateStruct = struct {
    pub const nested_public = "Public const in MyPrivateStruct";
};

pub fn getMyPrivatStruct() type {
    return MyPrivateStruct;
}
```
{{< /file >}}

{{< file "main.zig" >}}
```zig
const std = @import("std");
const expose = @import("expose.zig");

pub fn main() void {
    const ExposedPrivateStruct = expose.getMyPrivatStruct();
    const MyPublicStruct = expose.MyPublicStruct;
    std.debug.print("{s}\n", .{expose.shankar});
    std.debug.print("{s}\n", .{MyPublicStruct.nested_public});
    std.debug.print("{s}\n", .{ExposedPrivateStruct.nested_public});
}
```
{{< /file >}}

In the file _expose.zig_ the struct `MyPrivateStruct` is not exposed by `pub` keyword but since it is the return value of public function `getMyPrivatStruct()`, it gets exposed in the `main.zig` file when the fuction is called.

The use of `pub` keyword is quite straightforward. Any declaration that has to be exposed should have `pub` keyword preceeding it or should be returned by an exposed function.