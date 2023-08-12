---
title: "Reflector"
date: 2023-08-12T20:08:57+08:00
draft: false
tags: ["archlinux", "reflector"]
---

## 基础介绍

`refletor`的简单介绍可以参看[WIKI](https://wiki.archlinuxcn.org/wiki/Reflector?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DReflector_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno)。

这里部分进行摘录

> Reflector 是一个 Python 脚本；它可以从 Arch Linux Mirror Status 页面获取最新的镜像列表，然后筛选出最新的镜像并按速度排序，最后将结果写入到 /etc/pacman.d/mirrorlist 文件。


`arch`自带的`mirrorlist`并非一定适用，自定义筛选才能更适合自己。

## 官方介绍

[官方文档](https://xyne.dev/projects/reflector/)很简洁，但是也足够用了。

研究一下具体参数

```txt
$ reflector --help

usage: reflector [-h] [--connection-timeout n] [--download-timeout n]
                 [--list-countries] [--cache-timeout n] [--url URL]
                 [--save <filepath>] [--sort {age,rate,country,score,delay}]
                 [--threads n] [--verbose] [--info] [-a n] [--delay n]
                 [-c <country name or code>] [-f n] [-i <regex>] [-x <regex>]
                 [-l n] [--score n] [-n n] [-p <protocol>]
                 [--completion-percent [0-100]] [--isos] [--ipv4] [--ipv6]

retrieve and filter a list of the latest Arch Linux mirrors

options:
  -h, --help            show this help message and exit
  --connection-timeout n
                        The number of seconds to wait before a connection
                        times out. Default: 5
  --download-timeout n  The number of seconds to wait before a download times
                        out. Default: 5
  --list-countries      Display a table of the distribution of servers by
                        country.
  --cache-timeout n     The cache timeout in seconds for the data retrieved
                        from the Arch Linux Mirror Status API. The default is
                        300.
  --url URL             The URL from which to retrieve the mirror data in JSON
                        format. If different from the default, it must follow
                        the same format. Default:
                        https://archlinux.org/mirrors/status/json/
  --save <filepath>     Save the mirrorlist to the given path.
  --sort {age,rate,country,score,delay}
                        Sort the mirrorlist. "age": last server
                        synchronization; "rate": download rate; "country":
                        country name, either alphabetically or in the order
                        given by the --country option; "score": MirrorStatus
                        score; "delay": MirrorStatus delay.
  --threads n           Use n threads for rating mirrors. This option will
                        speed up the rating step but the results will be
                        inaccurate if the local bandwidth is saturated at any
                        point during the operation. If rating takes too long
                        without this option then you should probably apply
                        more filters to reduce the number of rated servers
                        before using this option.
  --verbose             Print extra information to STDERR. Only works with
                        some options.
  --info                Print mirror information instead of a mirror list.
                        Filter options apply.

filters:
  The following filters are inclusive, i.e. the returned list will only
  contain mirrors for which all of the given conditions are met.

  -a n, --age n         Only return mirrors that have synchronized in the last
                        n hours. n may be an integer or a decimal number.
  --delay n             Only return mirrors with a reported sync delay of n
                        hours or less, where n is a float. For example. to
                        limit the results to mirrors with a reported delay of
                        15 minutes or less, pass 0.25.
  -c <country name or code>, --country <country name or code>
                        Restrict mirrors to selected countries. Countries may
                        be given by name or country code, or a mix of both.
                        The case is ignored. Multiple countries may be
                        selected using commas (e.g. --country France,Germany)
                        or by passing this option multiple times (e.g. -c fr
                        -c de). Use "--list-countries" to display a table of
                        available countries along with their country codes.
                        When sorting by country, this option may also be used
                        to sort by a preferred order instead of
                        alphabetically. For example, to select mirrors from
                        Sweden, Norway, Denmark and Finland, in that order,
                        use the options "--country se,no,dk,fi --sort
                        country". To set a preferred country sort order
                        without filtering any countries. this option also
                        recognizes the glob pattern "*", which will match any
                        country. For example, to ensure that any mirrors from
                        Sweden are at the top of the list and any mirrors from
                        Denmark are at the bottom, with any other countries in
                        between, use "--country 'se,*,dk' --sort country". It
                        is however important to note that when "*" is given
                        along with other filter criteria, there is no
                        guarantee that certain countries will be included in
                        the results. For example, with the options "--country
                        'se,*,dk' --sort country --latest 10", the latest 10
                        mirrors may all be from the United States. When the
                        glob pattern is present, it only ensures that if
                        certain countries are included in the results, they
                        will be sorted in the requested order.
  -f n, --fastest n     Return the n fastest mirrors that meet the other
                        criteria. Do not use this option without other
                        filtering options.
  -i <regex>, --include <regex>
                        Include servers that match <regex>, where <regex> is a
                        Python regular express.
  -x <regex>, --exclude <regex>
                        Exclude servers that match <regex>, where <regex> is a
                        Python regular express.
  -l n, --latest n      Limit the list to the n most recently synchronized
                        servers.
  --score n             Limit the list to the n servers with the highest
                        score.
  -n n, --number n      Return at most n mirrors.
  -p <protocol>, --protocol <protocol>
                        Match one of the given protocols, e.g. "https" or
                        "ftp". Multiple protocols may be selected using commas
                        (e.g. "https,http") or by passing this option multiple
                        times.
  --completion-percent [0-100]
                        Set the minimum completion percent for the returned
                        mirrors. Check the mirrorstatus webpage for the
                        meaning of this parameter. Default value: 100.0.
  --isos                Only return mirrors that host ISOs.
  --ipv4                Only return mirrors that support IPv4.
  --ipv6                Only return mirrors that support IPv6.
```

这里专门记录几个可能会常用的几个参数
| param                                  | description                   |
| -------------------------------------- | ----------------------------- |
| `--save`                               | 指定保存路径                  |
| `--sort`                               | 排序，我们可以指定最新、最快  |
| `--threads`                            | 多线程，加快更新              |
| `-a --age`                             | 最近更新，`n`标识几小时内更新 |
| `-c --country`                         | 指定城市                      |
| `-f n, --fastest n`                    | 指定最快的镜像                |
| `-p <protocol>, --protocol <protocol>` | 指定协议 |


## 常用参数

- 最新的5个，按照速度排序

```shell
reflector --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
```


- 最新200个，指定协议，按照速度排序

```shell
reflector --latest 200 --protocol http,https --sort rate --save /etc/pacman.d/mirrorlist
```

- 指定城市，最新12小时更新，执行协议，按照速度排序

```shell
reflector --country France,Germany --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

- 中国，最新12小时内更新，支持`https`，保留10个

```shell
reflector --country China --latest 10 --sort --age 12 rate --protocol https --save /etc/pacman.d/mirrorlist
```



