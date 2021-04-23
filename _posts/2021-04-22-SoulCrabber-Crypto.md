---
title: HTB Cyber Apocalypse 2021 - SoulCrabber | Crypto
published: false
---

## [](#header-2)SoulCrabber

> Aliens heard of this cool newer language called Rust, and hoped the safety it offers could be used to improve their stream cipher.

> Given output: 1b591484db962f7782d1410afa4a388f7930067bcef6df546a57d9f873

> Given Script in RUST

```
use rand::{Rng,SeedableRng};
use rand::rngs::StdRng;
use std::fs;
use std::io::Write;

fn get_rng() -> StdRng {
    let seed = 13371337;
    return StdRng::seed_from_u64(seed);
}

fn rand_xor(input : String) -> String {
    let mut rng = get_rng();
    return input
        .chars()
        .into_iter()
        .map(|c| format!("{:02x}", (c as u8 ^ rng.gen::<u8>())))
        .collect::<Vec<String>>()
        .join("");
}

fn main() -> std::io::Result<()> {
    let flag = fs::read_to_string("flag.txt")?;
    let xored = rand_xor(flag);
    println!("{}", xored);
    let mut file = fs::File::create("out.txt")?;
    file.write(xored.as_bytes())?;
    Ok(())
}

```

### [](#header-3)Solution

After spending a couple hours setting up and learning the basics of Rust, I have learned how to mess with the code now.

The fact that they used a constant 13371337 for their rng function makes it actually not random at all. 

```
use rand::rngs::StdRng;
use rand::{Rng, SeedableRng};
use std::fs;
use std::io::Write;

fn get_rng() -> StdRng {
  let seed = 13371337;
  return StdRng::seed_from_u64(seed);
}

fn rand_xor(input: String) -> String {
  let mut rng = get_rng();
  return input
    .chars()
    .into_iter()
    .map(|c| format!("{:02x}", (c as u8 ^ rng.gen::<u8>())))
    .collect::<Vec<String>>()
    .join("");
}

fn main() -> std::io::Result<()> {
  let chars = vec![
    "a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s",
    "t", "u", "v", "w", "x", "y", "z", "_", "}", "0", "1", "2", "3", "4", "5", "6", "7", "8", "9",
  ];

  let flag = "CHTB{";
  for char in chars.iter() {
    let answer = flag.to_owned() + char;
    let xored = rand_xor(answer.to_string());
    if xored == "1b591484db" {
      println!("{}", answer);
    }
  }
  Ok(())
}
```

The difference between their script and mine is that I added a for loop in the main function to help me bruteforce the flag character by character manually.

Summary of the whole process:
1. Script loops through each character in my customized alphabet and appends to my given flag format.
2. Script compares each output to the 10 digits output I gave
3. Script prints out that one extra character once a match has been found
4. Add that extra character in flag variable
5. Add 2 more digits from the given _output.txt_
6. Repeat Steps 1 - 5 until all digits have been taken out from _output.txt_

I probably could have automated the process, but I really didn't want to spend more time figuring out how to do that in Rust.

Flag: CHTB{mem0ry_s4f3_crypt0_f41l} 
