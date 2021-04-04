---
title: ShaktiCon CTF 2021 - CrackMe  | Password
published: true
---

## [](#header-2)CrackMe

> I follow all the security rules, for example, I have ensured that my password is in lowercase, long enough, no spaces and with the format mydogbreed, myluckycolor, mycountrycode and underscores in between. 
See how cautious I am. Oh wait, did I leak something that I should not?
>Password Hash: 1bf575335ff6a3bad86bfa2c83a884fa018253483c7e5630629761a23523e963
>Add shaktictf{} to the password obtained

### [](#header-3)Solution

This challenge was pretty straightforward since they told you what the password consists of, and probably means _rockyou.txt_ isn't going to helpful here.
There is a lot of trial and error going on, but after quite some time I finally got the correct combination.
I used Python to help me create a wordlist that combines the [dog breed](https://sortmylist.com/reference/biology/dog_breeds.txt) (excluding breed names with parentheses) with [general colors](https://github.com/imsky/wordlists/blob/master/adjectives/colors.txt).
I had to get rid of the capitalization and empty spaces in both txt document.
```Python
//Python
file = open('Breeds.txt')

colors =['yellow', 'green', 'blue', 'violet', 'purple', 'gray', 'grey', 'red', 'black', 'white', 'orange', 'cyan', 'pink', 'silver', 'brown', 'gold', 'magenta', 'lime', 'amber', 'aqua', 'azure', 'amethyst', 'aquamarine', 'apricot', 'auburn', 'beige', 'bronze', 'buff', 'cardinal', 'cerise', 'chartreuse', 'coral', 'carmine', 'chocolate', 'cream', 'charcoal', 'copper', 'crimson', 'cinnamon', 'celadon', 'dark', 'denim', 'ebony', 'emerald', 'ecru', 'eggplant', 'fuchsia', 'goldenrod', 'hue', 'indigo', 'ivory', 'jade', 'jet', 'khaki', 'lavender', 'lilac', 'lemon', 'mauve', 'maroon', 'mustard', 'mahogany', 'olive', 'ocher', 'orchid', 'pumpkin', 'peach', 'pastel', 'puce', 'pewter', 'persimmon', 'rainbow', 'ruby', 'rose', 'russet', 'salmon', 'saffron', 'sapphire', 'scarlet', 'sepia', 'sienna', 'shade', 'shamrock', 'teal', 'tint', 'turquoise', 'topaz', 'terracotta', 'tangerine', 'umber', 'vermilion', 'viridian', 'wisteria']

for line in file:
    for color in colors:
        line = line.replace(" ", "")
        line = line.lower()
        line = line.strip()
        print(line + '_' + color + '_')
```

Afterwards, I copied the list of country codes (including "-" characters) from [https://countrycode.org/](this website) and paste them onto Excel. Isolate the country codes on Excel and paste it in a txt file.

Then, I used an [https://www.tunnelsup.com/hash-analyzer/](online hash analyzer) and found out that I was dealing with SHA2-256 here.

![image](https://user-images.githubusercontent.com/81070073/113497984-984f5d80-94bd-11eb-9077-394aedc707f8.png)

Finally, I used **hashcat** to combine the wordlist made in Python with the country code txt file and cracked the hash.

```Linux
//Linux
$hashcat -m 1400 -a 1 hash.txt python_wordlist.txt country_code.txt 
1bf575335ff6a3bad86bfa2c83a884fa018253483c7e5630629761a23523e963:dachshund_aquamarine_974
```

Flag: shakticon{dachshund_aquamarine_974}
