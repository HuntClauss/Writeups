# Movie Login 3 | Web

### Solution

From ctf we have got url to website with login panel and more restricted denylist.json than before with blacklisted characters.
##### denylist.json

```json
[
    "and",
    "1",
    "0",
    "true",
    "false",
    "/",
    "*",
    "=",
    "xor",
    "null",
    "is",
    "<",
    ">"
]
```

The same as before I slightly changed SQL logical statement and ended up with:<br />
> Login: **admin**<br />
> Password: **admin' OR 2 --**


### Flag

bcactf{gu3ss_th3r3s_n0_st0pp1ng_y0u!}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss