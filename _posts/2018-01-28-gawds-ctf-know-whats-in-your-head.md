---
layout: post
title: Gawds CTF/ Know whats in your head
desc: This challenge was filled with dummy flags and was quite frustrating. Looking at the description, it looks like the answer lies within the headers.
comments: true 
---

## Description

Try to see whats inside your head before moving forward.

[https://head.ctf.gawds.in](https://head.ctf.gawds.in)

This challenge was filled with dummy flags and was quite frustrating.

* * * 

## The Challenge

Looking at the description, it looks like the answer lies within the headers.

![Challenge](/public/assets/images/startup-knowyourhead.png) 

Like always, checking the `robots.txt` file, gave up the following info.

<pre>
### BEGIN FILE ###

User-agent: gawds-crawler
Disallow:

### END FILE ###
</pre>

From the robots.txt file, we can now infer that the `User-Agent` must be `gawds-crawler`.

Navigating to 
<pre>http://swisshawk.ctf.gawds.in/flag</pre>
 greets us with a <strong>Access Denied</strong> error.

Changing the method to `POST` also results in the same error.

I started to dig more.

I stumble upon `sitemap.xml`
<pre>
This XML file does not appear to have any style information associated with it. The document tree is shown below.
&lt;urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"&gt;
&lt;url&gt;
&lt;loc&gt;http://www.example.com/&lt;/loc&gt;
&lt;lastmod>2018-01-02</lastmod&gt;
&lt;changefreq>once</changefreq&gt;
&lt;priority>0.8</priority&gt;
&lt;/url&gt;
&lt;url&gt;
&lt;loc&gt;
http://www.example.com/53129cdb3222c675a3ab1d3763a7665e90e26aed
&lt;/loc&gt;
&lt;lastmod>2018-01-02</lastmod&gt;
&lt;changefreq>once</changefreq&gt;
&lt;priority>1.0</priority&gt;
&lt;/url&gt;
&lt;/urlset&gt;
</pre>

Visiting
<pre>https://head.ctf.gawds.in/53129cdb3222c675a3ab1d3763a7665e90e26aed</pre>

![Dummy1](/public/assets/images/knowyourhead-dummy1.png)

Checking the headers:

<pre>tryharderflag:flag{Alm0st_Reach4d_But_THis_is_A_DUmmY_Flag}</pre> 

This was quite pissing off! I decided to give last and final shot.


Looking at the hash string, it seemed like a sha-1 hash.

So, I performed sha-1 hash of `flag` and obtained 

`112f3a99b283a4e1788dedd8e0e5d35375c33747`

Visiting,
 <pre>https://head.ctf.gawds.in/112f3a99b283a4e1788dedd8e0e5d35375c33747</pre>

You get:

![Dummy2](/public/assets/images/knowyourhead-dummy2.png)

When we check the headers, we get:

<strong>Flag: flag{G00d_W0rk_AlWys_check_f0R_headers}</strong>
