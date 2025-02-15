---
title: ISSUE & FEATURE COLLECTION
date: YYYY-MM-DD HH:mm:ss
categories: [Tech Points, Features, Issues]
tags: [Technical, Functionality, Issue]
---


### 1 - issue: 解决访问github SSL_ERROR_SYSCALL问题

```
$ git push
fatal: unable to access 'https://github.com/gentleen/token-back-end.git/': OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443
```

push 代码的时候产生如上错误

```
git config --global http.sslBackend "openssl"
```

执行如上命令 解决

### 2 - issue: 解决AngualrJS不能设置Content-Type请求头问题

如 `transformRequest` 示例

```
$http({
    method: 'GET',
    url: 'rest/home/banners',
    headers: {
        "x-version": "h5-v1.0.0",
        "x-timestamp": currentdate,
        'Content-type': 'application/json'  
    },
    transformRequest:function(obj){
        var str=[];
        for(var p in obj){
            str.push(encodeURIComponent(p) + "-"+encodeURIComponent(obj[p]));
        }
        return str.join("&");
    }
    ... ...
});
```

### 3 - feature: Angular button throttle directive sample

```
import { Directive, EventEmitter, HostListener, OnInit, Output } from '@angular/core';
import { Subject, Subscription } from 'rxjs';
import { throttleTime } from 'rxjs/operators';

@Directive({
  selector: '[throttleClickDirective]'
})
export class ThrottleClickDirective implements OnInit {
    @Output() throttleClick = new EventEmitter();
    private clicks = new Subject();
    private subscription: Subscription;

    constructor() { }

    ngOnInit() {
      this.subscription = this.clicks.pipe(
          throttleTime(1000)
      ).subscribe(e => this.throttleClick.emit(e));
    }

    ngOnDestroy() {
      this.subscription.unsubscribe();
    }

    @HostListener('click', ['$event'])
    clickEvent(event) {
      event.preventDefault();
      event.stopPropagation();
      this.clicks.next(event);
    }
}

```

### 4 - issue: Running docker container : iptables: No chain/target/match by that name

```
iptables -t filter -F
```
```
iptables -t filter -X
```

- which indeeds clear all chains. One possible solution is to launch the docker daemon after the iptables setup script. Otherwise you will need to explicitly removes chains you're interested in.

- Then restart docker

```
systemctl restart docker
```
