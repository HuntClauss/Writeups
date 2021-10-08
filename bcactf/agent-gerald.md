# Agent Gerald | Web

### Solution

From ctf we have got url to website.

This challenge is very simple and everything you need to solve this is to change user-agent to **Agent Gerald**<br />
```bash 
$ curl "http://web.bcactf.com:49156/" -A "Agent Gerald"
```

### Flag

bcactf{y0u_h@ck3d_5tegos@urus_1nt3lligence}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss