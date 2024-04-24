---
title: "How I webscraped 1 minute stock data from tradingview"
datePublished: Tue Dec 22 2020 01:21:27 GMT+0000 (Coordinated Universal Time)
cuid: clmctwfxs00bc12nv6ra5ctqi
slug: how-i-webscraped-1-minute-stock-data-from-tradingview

---

After a long die-hard trying I managed to get 1 minute stock data for free. My previous tries were using selenium and beautifulsoup modules in python. But the data is highly obfuscated so I was not able to find the exact HTML element to scrape. If you manage to do this using selenium please comment below.

Probably the data is being loaded up as SVG which is why it isn't being seen in html inspect element.

![](https://miro.medium.com/v2/resize:fit:2000/1*7ojvVgXhycJuHbtKNbmFig.png align="left")

But...

After a look into Network section I found something interesting.

![](https://miro.medium.com/v2/resize:fit:3840/1*VzKxyHCAjgPuqZxyK9zXlQ.png align="left")

**websockets !!!**

After observing the Headers

![](https://miro.medium.com/v2/resize:fit:2000/1*L4gkZOXs1DkHwMsPcpoDGw.png align="left")

And Messages

![](https://miro.medium.com/v2/resize:fit:1400/1*mrlOOtjKIik471xb87m55w.png align="left")

I tried to send the same messages using websockets in python and it worked **:)** .

Code at [https://github.com/rushic24/tradingview-scraper](https://github.com/rushic24/tradingview-scraper).

## Limitations

We can only get previous 5000 ticks of data.