---
title: TryHackMe Challenge | Reversing ELF
published: true
---

## [](#header-2)Task 1: Crackme1

> Let's start with a basic warmup, can you run the binary?

### [](#header-3)Solution

Using the following command, I was able to make the given ELF file _crackme1_ executable and retrieve the flag.

```Linux
// Linux command
$ chmod +x crackme1
$ ./crackme1
flag
```

## [](#header-2)Task 2: Crackme2

> Find the super-secret password! and use it to obtain the flag

### [](#header-3)Solution

After making the given file _carackme2_ executable, I was prompted to enter a password following the file name.
Using the *strings* command, I was able to find the password.

```Linux
// Linux command
$ chmod +x crackme2
$ strings crackme2
$ ./crackme2 super_secret_password
flag
```

## [](#header-2)Task 3: Crackme3

> Use basic reverse engineering skills to obtain the flag

### [](#header-3)Solution

Using the *strings* command once more, I was able to find a base64 encoded string and I found the flag after decoding it.

```Linux
// Linux command
$ chmod +x crackme3
$ strings crackme3
$ echo "base64_encoded_string_here" | base64 -d
flag
```

## [](#header-2)Task 4: Crackme4

> Analyze and find the password for the binary?

### [](#header-3)Solution

To be continued ~

```
$ gdp crackme4
```

insert the table here
