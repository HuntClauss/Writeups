# Secure Zip | Forensic

### Solution

From ctf we have got chall.zip with unknown password.

In this challenge we need to crack password of zip archive. to achieve this I used tool called **John the Ripper**
```bash
$ zip2john chall.zip > hash.txt | cat hash.txt
chall.zip:$pkzip2$2*2*1*0*0*24*75ee*96ef*721bb447d02490edd8fb99f18b637079cfb44c39f72373d6c1ee85f0ed469bcec97c929a*2*0*40*34*75eef70d*0*42*0*40*75ee*96a7*a059a4b8146ea4acc61cfe3eab8663ba9c2e64f9bdeecfcfe49aedf18d4837717cd182c60846b08e0684e1725ec3654c047d7fac24efa18db0c874d7cb24b6ed*$/pkzip2$::chall.zip:flag.txt, homework.txt:chall.zip

$ john --show hash.txt
chall.zip:dogedoge::chall.zip:flag.txt, homework.txt:chall.zip
1 password hash cracked, 0 left

$ unzip -P dogedoge chall.zip
Archive:  chall.zip
 extracting: flag.txt                
 extracting: homework.txt
 
$ cat flag.txt
bcactf{cr4ck1ng_z1p_p455w0rd5_15_fun_a12ca37bdacef7}
```


### Flag

bcactf{cr4ck1ng_z1p_p455w0rd5_15_fun_a12ca37bdacef7}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss