# Home Automation | Web

### Solution

From ctf we have got url to website.

Objective of this challenge is to turn off the light on the website, but when we try to turn it off the error appears `You must be admin to turn off the lights. Currently you are "vampire"`.<br />
The first thought was I must change cookie value and as I suspected there was a cookie:<br />
> user: vampire <br />

So I changed it value to **admin** and refresh the page to see the flag<br />

### Flag

bcactf{c00k13s_s3rved_fr3sh_fr0m_th3_smart_0ven_cD7EE09kQ}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss