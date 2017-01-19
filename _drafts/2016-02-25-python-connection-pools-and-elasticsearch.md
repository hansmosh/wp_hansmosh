---
ID: 63
post_title: >
  python connection pools and
  elasticsearch
author: hansmosh
post_date: 2016-02-25 13:45:07
post_excerpt: ""
layout: post
permalink: http://blog.hansmosh.com/?p=63
published: false
---
default python es client uses urillib3

test: save to es once per second, this works with 10 threads, for every thread over that i get "[urllib3.connectionpool] Connection pool is full, discarding connection: 192.168.100.95"

switch to requests: "[requests.packages.urllib3.connectionpool] Connection pool is full, discarding connection: 192.168.100.95"

Increase maxsize parameter to Urllib3HttpConnection