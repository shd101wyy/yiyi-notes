---
tags: []
created: 2020-08-03T14:19:48.004Z
modified: 2020-08-03T14:28:23.064Z
---
# 写了个简易的微博用户爬取 TamperMonkey 脚本

逻辑可以自行修改

爬取完成后运行 

```javascript
WUSPrintMatchedUsers()  // 得到 match 的用户
WUSClearCaches()        // 删除缓存
```

脚本 👇 

```javascript
// ==UserScript==
// @name         Weibo user search
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  根据正则表达式来微博找人
// @author       shd101wyy
// @match        https://s.weibo.com/*
// @match        https://weibo.com/*
// @grant        none
// ==/UserScript==

(async function() {
    'use strict';

    // 修改一下正则表达式来进行过滤
    // 设置为 null 则不进行匹配
    const personInfoRegExp = null // 个人基本信息匹配  例如 /(其它|北京|魔羯|1994|4月|1日)/
    const weiboRegExp = null      // 微博内容匹配     例如 /(老师好我叫何同学|何同学)/
    const relation = "or"        // "and" or "or"
    const perPageWaitForTimeout = 6000;

    const localStorageUsers = "t:users";
    const localStorageCrawledList = "t:crawledList";

    const parseQueryString = function( queryString ) {
        let params = {}, queries, temp, i, l;
        // Split into key/value pairs
        queries = queryString.split("&");
        // Convert the array of strings into an object
        for ( i = 0, l = queries.length; i < l; i++ ) {
            temp = queries[i].split('=');
            params[temp[0]] = decodeURIComponent(temp[1]);
        }
        return params;
    };

    const waitFor = function(t) {
        return new Promise((resolve, reject)=> {
            setTimeout(()=> {
                return resolve();
            }, t)
        })
    }

    const waitForSelector = async function(selector, t=500) {
        if (!!document.querySelector(selector)) {
            return;
        } else {
            console.log("未找到 " + selector);
            await waitFor(t);
            return await waitForSelector(selector, t);
        }
    }

    const addUserToLocalStorage = function (username) {
        let users = {};
        try {
            users = JSON.parse(localStorage.getItem(localStorageUsers) || "{}")
        } catch(error) {
            users = {}
        }
        users[username] = window.location.origin + window.location.pathname;
        localStorage.setItem(localStorageUsers, JSON.stringify(users));
    }

    const markUrlAsCrawled = function(url=window.location) {
        let crawledList = [];
        try {
            crawledList = JSON.parse(localStorage.getItem(localStorageCrawledList) || "[]");
        } catch(error) {
            crawledList = [];
        }
        const sanitizedUrl = url.origin + url.pathname
        if (crawledList.indexOf(sanitizedUrl) < 0) {
            crawledList.push(sanitizedUrl);
        }

        localStorage.setItem(localStorageCrawledList, JSON.stringify(crawledList));
    }

    window.WUSPrintMatchedUsers = function() {
        console.log(JSON.parse(localStorage.getItem(localStorageUsers)));
    }

    window.WUSClearCaches = function() {
        localStorage.removeItem(localStorageUsers);
        localStorage.removeItem(localStorageCrawledList);
    }



    console.log("开始运行微博搜人脚本");
    if (location.origin.match(/s\.weibo\.com/)) {
        console.log("进入搜索页面");
        await waitFor(4000);
        const infos = document.querySelectorAll(".info");
        const hrefs = [];
        for (let i = 0; i < infos.length; i++) {
            const info = infos[i];
            const anchor = info.querySelector(".name");
            const href = anchor.href || "";
            if (href) {
                hrefs.push(href);
            }
        }
        let crawledList = [];
        try {
            crawledList = JSON.parse(localStorage.getItem(localStorageCrawledList) || "[]");
        } catch(error) {
            crawledList = [];
        }

        for (let i = 0; i < hrefs.length; i++) {
            const href = hrefs[i];
            const url = new URL(href);
            if (crawledList.indexOf(url.origin + url.pathname) < 0) {
                window.open(href, "_blank");
                markUrlAsCrawled(url);
                await waitFor(perPageWaitForTimeout);
            }
        }

        // check next page
        const nextPageBtn = document.querySelector("a.next");
        if (nextPageBtn && nextPageBtn.href) {
            await waitFor(perPageWaitForTimeout);
            window.open(nextPageBtn.href, "_blank");
        }
        window.close();
    } else if (location.pathname.match(/^\/u\/\d+$/) || location.pathname.match(/^\/[a-z\d]+$/i)) {
        console.log("进入用户主页");
        setTimeout(()=> {
            window.close();
        }, 30000)
        await waitFor(4000);
        await waitForSelector(".username");
        await waitForSelector(".PCD_person_info");
        await waitForSelector(".WB_frame_c");
        const username = document.querySelector(".username").innerText;
        const personInfo = document.querySelector(".PCD_person_info");
        const weiboList = document.querySelector(".WB_frame_c")
        const p = (personInfoRegExp && personInfo.innerText.match(personInfoRegExp))
        const w = (weiboRegExp && weiboList.innerText.match(weiboRegExp))
        const flag = relation === "or" ? w || p : w && p;
        if (flag) {
            console.log("成功匹配");
            alert("匹配成功");
            addUserToLocalStorage(username);
            markUrlAsCrawled();
        } else {
            console.log("未成功匹配");
            markUrlAsCrawled();
            window.close();
        }
    }
})();
```