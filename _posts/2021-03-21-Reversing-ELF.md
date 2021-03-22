---
title: TryHackMe Challenge | Reversing ELF
published: true
---
# [](#header-1)Task 1: Crackme1

## [](#header-2)Prompt

> Let's start with a basic warmup, can you run the binary?

### [](#header-3)Solution

Using the following command, I was able to make the given ELF file executable and retrieve the flag.

```Linux
// Linux command
$ chmod + x crackme1
$ ./crackme1
*flag*
```
