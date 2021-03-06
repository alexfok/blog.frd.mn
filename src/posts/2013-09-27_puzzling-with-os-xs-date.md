---
title: "Puzzling with OS X's date command"
date: 2013-09-27T10:00:00.000Z
disqus_id: 6
slug: puzzling-with-os-xs-date
---

Currently I am building a *Bash* based toolkit to [troubleshoot and analyze SSL/TLS](https://github.com/frdmn/ssltools) certificates and certificate requests. I ran into a confusing problem and likewise interesting problem during the development:

```shell
date -j -f '%b %d %T %Y %Z' 'Aug  7 07:09:42 2014 GMT' +%s
```

> ```
> 1407395382
> ```

```shell
date -j -f '%b %d %T %Y %Z' 'Oct 27 17:35:57 2013 GMT' +%s
```

> ```
> Failed conversion of ``Oct 27 17:35:57 2013 GMT'' using format ``%b %d %T %Y %Z''
> date: illegal time format
> usage: date [-jnu] [-d dst] [-r seconds] [-t west] [-v[+|-]val[ymwdHMS]] ...
>             [-f fmt date | [[[mm]dd]HH]MM[[cc]yy][.ss]] [+format]
> ```

As you can see the first statement returned our timestamp without any problem, but the second one fails with a _date: illegal time format_ message even though we use the same command options/syntax. Why is that? The answer is rather simple: month and day representations have to be your in system's nationality/language!

So if you change the `Oct` to `Okt` (I am running a german OS X), the conversion works without any problems.

# How to solve this?

Either you change your date informations to the systems nationality, or you do this simple trick:

```shell
LANG=C date -j -f '%b %d %T %Y %Z' 'Oct 27 17:35:57 2013 GMT' +%s
```

> ```
> 1382895357
> ```

Putting a `LANG=C` in front of the command tells `date` to act normal and not OS X-like so you don't run into this problem as well.

Credits to __drkshadow__ out of the [#bash](irc://irc.freenode.net/#bash) who assisted me here.
