# Countdown Timer | Web

### Solution

From ctf we have got url to website.

After entering the website we can see description how countdown timer works and how to get flag.<br />
> Flag will be displayed after countdown reaches 0. We can specify how long countdown will be, but minimal allowed value is 100 days. (obviously this is too long)<br />

There were no requests in the browser networking tab after count down started, so I assume timer is running locally with JS.

```js
var time = 100000;
var minTime = 100;
var daysInput = document.getElementById("countdownDays");
var counter;
document.getElementById("startButton").onclick = function () {
    startCountDown();
};
document.getElementById("minTimeHeader").innerHTML = "Minimum Time: " + minTime + " days";

daysInput.min = minTime;

function startCountDown() {
    clearInterval(counter);
    if (daysInput.value < minTime) {
        daysInput.value = minTime;
    }
    time = daysInput.value * 24 * 60 * 60;
    counter = setInterval(countdown, 1000);
}

function countdown() {
    time -= 1;
    if (time <= 0) {
        getFlag();
        clearInterval(counter);
        return;
    }
    var numdays = Math.floor(time / 86400);
    var numhours = Math.floor((time % 86400) / 3600);
    var numminutes = Math.floor(((time % 86400) % 3600) / 60);
    var numseconds = ((time % 86400) % 3600) % 60;
    document.getElementById("remainingTime").innerHTML = numdays + " Days " + numhours + " Hours " + numminutes + " Minutes " + numseconds + " Seconds";
}

//This is not part of the intended solve path, ignore this
function getFlag() {
    var _0x2636 = ['atob', '5117oZMdEn', 'then', 'VjIxak9WQlJQVDA9', 'Time\x20is\x20up!\x20Flag:\x20', 'text', '2myDwax', '80990TIAiao', '159475uJlWpn', '486144SCONMd', '1ZGbuyJ', '70599pvkkee', 'getElementById', '39313hGSvrt', 'remainingTime', '2ryZPVf', '1AonKEy', '133880SpkfyF'];

    function _0x91fe(_0x432e25, _0x1d1f40) {
        _0x432e25 = _0x432e25 - 0x199;
        var _0x26367c = _0x2636[_0x432e25];
        return _0x26367c;
    }

    var _0x54df47 = _0x91fe;
    (function (_0x5ee5d0, _0x1eb0fc) {
        var _0x3fc014 = _0x91fe;
        while (!![]) {
            try {
                var _0x1bb0d9 = parseInt(_0x3fc014(0x1a9)) + parseInt(_0x3fc014(0x19d)) * parseInt(_0x3fc014(0x19b)) + parseInt(_0x3fc014(0x1a6)) + parseInt(_0x3fc014(0x19f)) * -parseInt(_0x3fc014(0x19c)) + -parseInt(_0x3fc014(0x1a8)) * -parseInt(_0x3fc014(0x1a5)) + -parseInt(_0x3fc014(0x199)) * -parseInt(_0x3fc014(0x1a4)) + -parseInt(_0x3fc014(0x1a7));
                if (_0x1bb0d9 === _0x1eb0fc) break; else _0x5ee5d0['push'](_0x5ee5d0['shift']());
            } catch (_0x27a98) {
                _0x5ee5d0['push'](_0x5ee5d0['shift']());
            }
        }
    }(_0x2636, 0x2892d), fetch('/' + window['atob'](window[_0x54df47(0x19e)](window[_0x54df47(0x19e)](window['atob'](window['btoa'](window['atob'](_0x54df47(0x1a1))))))))[_0x54df47(0x1a0)](_0x192f21 => _0x192f21[_0x54df47(0x1a3)]())['then'](_0x1b849e => {
        var _0xbb5c79 = _0x54df47;
        time <= 0x0 && (document[_0xbb5c79(0x1aa)](_0xbb5c79(0x19a))['innerHTML'] = _0xbb5c79(0x1a2) + _0x1b849e);
    }));
}
```

After quick look into code we can see function called **getFLag()** but comment above suggest is not intended solution. (and function is obfuscated, so maybe search some simpler way)<br />

In my opinion best approach was to find where function **getFlag()** was called. My attention focused on **countdown()** function, and to be more precise on **if** statement. To get flag variable *time* must reach 0, so in browser console I typed `time = 0` and that's all.

### Flag

bcactf{1_tH1nK_tH3_CtF_w0u1D_b3_0v3r_bY_1O0_dAy5}

#### Credits

- Writeup by [HuntClauss](https://ctftime.org/user/106464)
- Solved by [HuntClauss](https://ctftime.org/user/106464)
- WaletSec 2021

#### License

**CC BY 4.0** WaletSec + HuntClauss