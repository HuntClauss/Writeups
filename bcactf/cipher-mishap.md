# Cipher Mishap | Crypto


### Solution

From ctf we have got text.txt file.

##### text.txt
```txt
126-Y, 113-N, 122-N, 130-N, 117-N, 107-N, 137, 114-N, 127-Y, 137, 113-Y, 104-N, 131-N, 110-N, 137, 105-Y, 110-N, 110-N, 121-Y, 137, 131-Y, 114-N, 112-N, 110-N, 121-N, 110-N, 125-N, 110-N, 137, 114-Y, 121-N, 126-N, 127-N, 110-N, 104-N, 107-N
```

This was a bit tricky, cause there was some guessing, but rather simple.<br />

I assume that these numbers are in base 8 system, because the highest element of all numbers is 7. At first I didn't know what this '-Y' and '-N' means, but I realized that after converting all numbers to ASCII characters I have got only upper case chars. Next hint was character behind *137* -> *_*. It's make sense, because *_* don't have upper/lower case version.<br />

After all of this I wrote simple python script to convert this text to ASCII form.
```py
cip = '126-Y, 113-N, 122-N, 130-N, 117-N, 107-N, 137, 114-N, 127-Y, 137, 113-Y, 104-N, 131-N, 110-N,' \
      ' 137, 105-Y, 110-N, 110-N, 121-Y, 137, 131-Y, 114-N, 112-N, 110-N, 121-N, 110-N, 125-N, 110-N, 137, 114-Y, 121-N, 126-N, 127-N, 110-N, 104-N, 107-N'

cip = cip.replace('137', '_')
cip = [i for i in cip.split(', ')]

result = ''

for v in cip:
    isUpper = True if v[-1] == 'Y' else False
    v = v[:-2]
    if v:
        result += chr(int(v, 8)) if isUpper else chr(int(v, 8)).lower()
    else:
        result += '_'

print(result)
```

Running this script result with this:
```txt
Vkrxog_lW_Kdyh_EhhQ_Yljhqhuh_Lqvwhdg
```

From the ctf description I know that at first this was a caesar cipher, so I found [this](https://www.dcode.fr/caesar-cipher) website that gives me decrypted text:
```txt
Should_iT_Have_BeeN_Vigenere_Instead
```


### Flag

bcactf{Should_iT_Have_BeeN_Vigenere_Instead}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss