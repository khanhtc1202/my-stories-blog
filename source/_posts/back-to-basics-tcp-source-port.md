---
title: Back-to-basics: Source port in TCP connection 
date: 2020-03-06 00:41:03
thumbnail: 'https://i.imgur.com/OSOKsx4.gif'
tags: technical
---

When making a TCP request, you need to specify the IP and the port. The combination is a socket. Some port is quite famous, such as 80 for web, 22 for ssh,... But have you ever wonder about the number of port from your end?
 
![TCP Connection](https://i.imgur.com/OSOKsx4.gif)
<!-- more -->

## The experiment

The idea is coming from `javax.servlet.ServletRequest`, I saw the method `getRemotePort` which will `Returns the Internet Protocol (IP) source port of the client or last proxy that sent the request.`. That makes me wonder which port will be used when fire a TCP connections.

I make a [simple program](https://github.com/bachnxhedspi/Head-First-Servlets-and-JSP/tree/master/TCP-source-port), it will prints out the source port of a connection.

Open `http://localhost:8080/display-local-port/` by the browser, the pod is a big value, and sometimes changed (54220 or something). 

![Example](https://i.imgur.com/q1syUgZ.png)

## The investigation

It turns out the port is assigned by the OS, it calls [Ephemeral port](https://en.wikipedia.org/wiki/Ephemeral_port).

You can specify the port from origin also, for example with curl by using `--local-port` option.

```
$ curl http://localhost:8080/display-local-port/ --local-port 2333-2444

<html>
<body>
<h1 align="center">Got the request information</h1>
<p>
    <br>At host: 0:0:0:0:0:0:0:1<br>At address: null<br>At port: 2333
</p>
</body>
</html>%
``` 

## What to go from here

It leads to a new question, how many connection a server can handle. As in Linux it is a specific number (`Many Linux kernels use the port range 32768 to 61000`), so is it limited by opening ports left?

It turns out more complicated, as discussions in [here](https://superuser.com/questions/251596/is-there-a-hard-limit-of-65536-open-tcp-connections-per-ip-address-on-linux?answertab=votes&fbclid=IwAR2g7OrnVRsPjms-Lbit3MAcXAFMVr8tDQiGZKwL0KFjt2P6Eg7AwfXvOcA#tab-top)
```
You may be thinking of number of ports. There are 65536 ports available in the TCP in the current versions of IPv4. This is not just a Linux limitation, its part of the protocol. Your IP address identifies your machine, and the port identifies a program on your machine.

But, the number of connections isn't limited by that. A connection consists of 5 pieces of info, in geek speak a 5-tuple. It is determined by protocol (TCP, UDP), local IP address and port, and remote IP address and port. So, take a webserver. It can service many connections on the same port (most likely 80). Your webserver can even support multiple connections to the same client machine. Say, you're connecting to google.com from two windows. Your machine will pick an unused port for each connection. So, google's server will have to keep track of (TCP, google.com, 80, yourmachine, someport1) and (TCP, google.com, 80, yourmachine, someport2). At some point you'd bump into limits, but it's not a hard limit, and is very system dependent.

And yes, each socket is a file descriptor, but not all machines use shorts for the fd table. On my system, a not tuned at all system, cat /proc/sys/fs/file-max gives 323997. I'm sure I could boost it if I needed to.

So, there is a limit of 65336, but it has to do with addressing, not number of connections. Number of connections is limited, but more by system config, and how much memory it has.
```

Still questioning myself, will update if I found the answer.
