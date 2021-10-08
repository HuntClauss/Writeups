# Sailing Thru Decryption | Crypto

### Solution

From ctf we have got image.png file.

Image file contains flags used by navy to communicate. With help of [this](https://www.allstarflags.com/facts/signal-code-flags/) website, I manually decoded all flags and ended up with this:
```txt
011001110110101001110011011011010111011101110011011110110011000101111000010111110110111100110001011010110101111101111000001101000111000001110010010111110110110000110011011110010011010001101010011011100011111101111101

Tango
Hotel
Echo
Kilo
Echo
Yankee
India
Sierra
Foxtrot
Hotel
Sierra
Kilo
Delta
November
```

Binary was just encoded ASCII: 
```txt
gjsmws{1x_o1k_x4pr_l3y4jn?}
```

Next thing that I think of was to write down first letters of decoded words, and that's ended up with some cipher key:
```txt
Thekeyisfhskdn
```

After some searching I found something called Vigenère cipher, and my findings result with decrypted flag.

### Flag

bcactf{1s_h1s_n4me_g3r4rd?}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss