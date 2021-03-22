---
title: TryHackMe Challenge | Reversing ELF
published: true
---

# [](#header-1)Task 1: Crackme1

## [](#header-2)Prompt

> Let's start with a basic warmup, can you run the binary?

### [](#header-3)Solution

Using the following command, I was able to make the given ELF file _crackme1_ executable and retrieve the flag.

```Linux
// Linux command
$ chmod + x crackme1
$ ./crackme1
flag
```

# [](#header-1)Task 2: Crackme2

## [](#header-2)Prompt

> Find the super-secret password! and use it to obtain the flag

### [](#header-3)Solution

After making the given file _carackme2_ executable, I was prompted to enter a password following the file name.
Using the *strings* command, I was able to find the password.

```Linux
// Linux command
$ strings Crackme2
$ ./crackme2 super_secret_password
flag
```
