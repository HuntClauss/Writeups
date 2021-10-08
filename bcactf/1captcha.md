# 1Captcha | Miscellaneous

### Solution

From ctf we have got challenge.ts file and url to website with captcha and documentation how it works.<br />
#### [captcha documentation](./1captcha-docs.md)
##### challenge.ts

```ts
import {NextApiRequest, NextApiResponse} from 'next';
import {CompactEncrypt} from 'jose/jwe/compact/encrypt';
import {compactDecrypt} from 'jose/jwe/compact/decrypt';
import {createCanvas, loadImage} from 'canvas';
import randomColor from 'randomcolor';

const challengeKey = Buffer.from(process.env.CAPTCHA_CHALLENGE_KEY!, "hex");
const encoder = new TextEncoder();
const decoder = new TextDecoder();
const stages = 20;

interface Challenge {
    stage: number;
    expected: boolean;
    correct: boolean[];
    expires: number;
}

function randomBool(): boolean {
    return Math.random() > 0.5;
}

function timeLimit(stage: number) {
    if (stage === 0) return 120;
    if (stage === 1) return 30;
    return Math.min(Math.max(2, 13 - stage), 10);
}

const image1 = loadImage("1.png");
const imagel = loadImage("l.png");

async function generateImage(input: boolean): Promise<string> {
    const canvas = createCanvas(64, 64);
    const ctx = canvas.getContext("2d");
    const lightBackground = randomBool();
    ctx.fillStyle = randomColor({luminosity: lightBackground ? "light" : "dark"});
    ctx.fillRect(0, 0, 64, 64);
    ctx.globalCompositeOperation = "destination-out";
    ctx.drawImage(input ? (await image1) : (await imagel), Math.floor(Math.random() * 48), Math.floor(Math.random() * 32));
    ctx.globalCompositeOperation = "destination-over";
    ctx.fillStyle = randomColor({luminosity: lightBackground ? "dark" : "light"});
    ctx.fillRect(0, 0, 64, 64);
    return canvas.toDataURL();
}

export default async (req: NextApiRequest, res: NextApiResponse) => {
    let stage = 0;
    if (typeof req.headers["x-captcha-challenge"] === "string") {
        let challenge: Challenge;
        try {
            const {plaintext} = await compactDecrypt(req.headers["x-captcha-challenge"], challengeKey);
            challenge = JSON.parse(decoder.decode(plaintext));
        } catch (e) {
            res.status(400);
            res.json({error: "Unable to parse challenge token"});
            return;
        }

        if (challenge.expires < Date.now() / 1000) {
            res.status(400);
            res.json({error: "Challenge expired"});
            return;
        }

        const answer: unknown = req.body.answer;
        if (typeof answer !== "object" || !(answer instanceof Array)) {
            res.status(400);
            res.json({error: "answer must be an array"});
            return;
        }
        if (answer.length !== 9) {
            res.status(400);
            res.json({error: "invalid answer length"});
            return;
        }
        if (answer.findIndex((item, index) => {
            return item !== (challenge.correct[index] === challenge.expected);
        }) >= 0) {
            res.status(400);
            res.json({error: "Incorrect"});
            return;
        }

        stage = challenge.stage + 1;
    }

    if (stage >= stages) {
        res.json({done: true, flag: process.env.CAPTCHA_FLAG});
        return;
    }

    // Generate a new challenge
    let correct = [];
    for (let i = 0; i < 9; ++i) {
        correct.push(randomBool());
    }
    const time = timeLimit(stage);
    const challenge = {stage, expected: randomBool(), correct, expires: Date.now() / 1000 + time};
    const jwe = await new CompactEncrypt(encoder.encode(JSON.stringify(challenge))).setProtectedHeader({
        alg: "A128KW",
        enc: "A256GCM"
    }).encrypt(challengeKey);
    res.json({
        challenge: jwe,
        stage,
        stages,
        expected: await generateImage(challenge.expected),
        images: await Promise.all(challenge.correct.map(input => generateImage(input))),
        time
    });
};
```

---

In this challenge we need to complete 20 stages of captcha to get a flag.<br />
Our team had few ideas how to solve this, but we decide to try one of the simplest - compare relative positions of pixels from expected image and 9 given.<br />
In the process, I found out that image of `1` and `l` have different height (one of these has 21 and other 20 pixels height). Knowing that I have created algorithm to find out the highest pixel position in char.<br /><br />

This is the python script I ended up to solve 20 stages of captcha in just a couple seconds.
```py
import base64
import io
import requests
from PIL import Image


def getCharRectangle(data):
    expected = Image.open(io.BytesIO(base64.b64decode(data[22:])))
    pixels = expected.load()

    background_color = pixels[0, 0]

    highest_pos = None

    for y, x in ((ty, tx) for ty in range(64) for tx in range(64)):
        if pixels[x, y] != background_color:
            highest_pos = (x, y)
            break

    if pixels[highest_pos[0] - 3, highest_pos[1] + 22] != background_color:
        return 21
    return 20


url = 'http://misc.bcactf.com:49158'

r = requests.post(f"{url}/api/challenge")
resp = r.json()
chall = resp['challenge']

while True:
    expected_height = getCharRectangle(resp['expected'])
    answer = [False] * 9

    for i in range(9):
        if getCharRectangle(resp['images'][i]) == expected_height:
            answer[i] = True

    r = requests.post(f"{url}/api/challenge", headers={"X-Captcha-Challenge": chall}, json={"answer": answer})
    resp = r.json()
    print(answer, resp)
    chall = resp['challenge']
```

Running this script ended up with long flag:
```bash
{'done': True, 'flag': 'bcactf{synthetic_cdo_returns_111l1l1l1l1l1l111l1ll11l1lll1l1l1l1llll1111lll1l1l1l1l}'}
```

### Flag

bcactf{synthetic_cdo_returns_111l1l1l1l1l1l111l1ll11l1lll1l1l1l1llll1111lll1l1l1l1l}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss