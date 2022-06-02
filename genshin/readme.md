# Genshin Impact Stats

> **WARNING!**\
> This widget is far from ready out-of-the-box,
> it requires two automate flows
> and optionally a web server to function properly.

A widget to show:
- commissions
- weekly bosses
- resin
- realm currency

## Preview

<img src="preview.jpg" width="540px"></img>

## Setup

### With web server
1. Set up a web server to get the data from hoyolab.\
   You can use [mine][server] which requires nodejs,
   or [write your own](server.md).

2. Import the `resin_*` Automate flows into Automate,
   and set the request URLs of their http blocks to your server.

3. Import the komponent into the Kustom app you are using,
   and change the shortcut action of the refresh button to
   start the `resin_once` flow.
   (content > buttons > refresh > touch)

4. Start the `resin_again` flow to make the widget
   auto-refresh.

#### Why?
**Can't you just make the request the server makes with Automate?**

Technically, yes, it is able to generate the required md5
checksums, but I found it would be easier to just let a
dedicated server handle that, since I already have a domain
and a vps.

### Without web server

1. Get your cookies from Hoyolab (on desktop):
   Go to [your battle chronicle][bc], open the developer console,
   and run `copy(document.cookie)`.
   You can now paste your cookies and send them to your phone.

2. Import the `resin_standalone_*` Automate flows into Automate,
   and add your info to the http blocks:
   - in the url, replace `uid` with your uid,
     and `os_serv` with one of the following,
     based on the first character of your uid:
     - 6: `os_usa`
     - 7: `os_euro`
     - 8: `os_asia`
     - 9: `os_cht`
   - in the request headers, the `Cookie` field to your cookies.
   

3. Import the komponent into the Kustom app you are using,
   and change the shortcut action of the refresh button to
   start the `resin_standalone_once` flow.
   (content > buttons > refresh > touch)

4. Start the `resin_standalone_again` flow to make the widget
   auto-refresh.



[server]: https://github.com/AroLeaf/resin-server
[bc]: https://act.hoyolab.com/app/community-game-records-sea/index.html#/ys