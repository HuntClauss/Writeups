# Movie Login 2 | Web

### Solution

From ctf we have got url to website with login panel and denylist.json with blacklisted characters.
##### denylist.json

```json
[
    "1",
    "0",
    "/",
    "="
]
```

I tried same schema of SQL injection as in challenge before, but change logical statement:<br />
> Login: **admin**<br />
> Password: **admin' OR 3>2 --**

### Flag

bcactf{h0w_d1d_y0u_g3t_h3r3_th1s_t1m3?!?}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss