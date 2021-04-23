---
title: HTB Cyber Apocalypse 2021 - SoulCrabber2 | Crypto
published: false
---

## [](#header-2)SoulCrabber2

> Aliens realised that hard-coded values are bad, so added a little bit of entropy.

> Given output: 418a5175c38caf8c1cafa92cde06539d512871605d06b2d01bbc1696f4ff487e9d46ba0b5aaf659807

> Given Script in RUST

```
use rand::{Rng,SeedableRng};
use rand::rngs::StdRng;
use std::fs;
use std::io::Write;
use std::time::SystemTime;

fn get_rng() -> StdRng {
    let seed = SystemTime::now()
        .duration_since(SystemTime::UNIX_EPOCH)
        .expect("Time is broken")
        .as_secs();
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

The only difference between this script and the previous script is that the aliens got smarter by hiding their rng seed using the EPOCH/UNIX Timestamp, but it's still just a constant.

Once I find that constant, it's checkmate.

This challenge required me to learn how to make a for loop which turnt out to be a lot more complicated than I thought even though I made one in the previous exercises.

I had to disassemble get_rng and rand_xor, move their contents inside main(), and make a loop inside main() that will loop through potential timestamps.

![image](https://user-images.githubusercontent.com/81070073/115827038-9abc1d80-a3c0-11eb-9fbb-03400daf3f5a.png)

Here tells me that the timestamp could be somewhere around 4/16/2021 4:32 AM, and that will help me define my range for the loop.

```
use rand::rngs::StdRng;
use rand::{Rng, SeedableRng};
use std::fs;
use std::io::Write;

fn main() -> std::io::Result<()> {
  for seed in 1618000000..1618600000 {
    let mut rng = StdRng::seed_from_u64(seed);
    let flag = "CHTB{";
    let xored = flag
      .chars()
      .into_iter()
      .map(|c| format!("{:02x}", (c as u8 ^ rng.gen::<u8>())))
      .collect::<Vec<String>>()
      .join("");
    if xored == "418a5175c3" {
      println!("{}", seed);
    }
  }
  Ok(())
}

//seed: 1618179277
```

After getting the seed as 1618179277, it's time to do the same thing I did in [SoulCrabber1](https://damoneer.github.io/hacker-blog/SoulCrabber-Crypto).

Flag: CHTB{cl4551c_ch4ll3ng3_r3wr1tt3n_1n_ru5t}

![image](https://user-images.githubusercontent.com/81070073/115827849-ba077a80-a3c1-11eb-9614-20e8e7524264.png)

Trying to convert the time back to our normal date and time format, unexpectedly, the time/date is way off comparing to what was shown to us in the metadata.

