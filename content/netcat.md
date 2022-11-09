---
Title: Netcat
Description: A cheat sheet for Netcat related items.
Author: Jack Szwergold
Date: 2015-10-17
Robots: noindex,nofollow
Template: index
---

#### Create a simple one-line web server with Netcat.

First, create a simple `index.html` file like this:

    echo Hello world\! > index.html

Now create the web server with Netcat like this; it will be available on `localhost` at port `8000`:

    while true; do { echo -e "HTTP/1.1 200 OK\r\n"; cat index.html; } | nc -l 8000; done

Or you could just have all of that tossed into one large one liner like this:

    while true; do { echo -e "HTTP/1.1 200 OK\r\n"; echo -e Hello world\!; echo -e "\r\n"; } | nc -l 8000; done

Then just check the URL in a web browser like this:

    localhost:8000

And you should see the contents of `index.html` (aka: “Hello world!”) show up in your browser.
