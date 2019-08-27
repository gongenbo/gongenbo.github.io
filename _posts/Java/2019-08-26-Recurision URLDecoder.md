---
layout: post
title: Recurision URLDecoder IllegalArgumentException's Solution
categories: Java
description: IllegalArgumentException URLDecoder
---
## Proble description

I am getting this exception while trying to decoder a url from using a Recursion Method in Java.

Here is the url
```aidl
https%3a%2f%2fpos.baidu.com%2fhcjm%3fconwid%3d320%26conhei%3d270%26rdid%3d4295444%26dc%3d3%26exps%3d110011%26psi%3dd75f47ad5ff815c2e06e410d8173b5f8%26di%3du4295444%26dri%3d0%26dis%3d3%26dai%3d1%26ps%3d0x0%26enu%3dencoding%26dcb%3d___adblockplus%26dtm%3dHTML_POST%26dvi%3d0.0%26dci%3d-1%26dpt%3dnone%26tsr%3d0%26tpr%3d1566471762118%26ti%3d%25E5%259B%25BD%25E9%2599%2585%25E6%2596%25B0%25E9%2597%25BB%252C%25E5%2585%25A8%25E7%2590%2583%25E5%258A%25A8%25E6%2580%2581%252C%25E6%259C%2580%25E6%2596%25B0%25E6%25B6%2588%25E6%2581%25AF%252C%25E5%2586%259B%25E4%25BA%258B%252C%25E5%2586%259B%25E6%2583%2585%252C%25E5%2585%25B5%25E5%2599%25A8%252C%25E7%25A4%25BE%25E4%25BC%259A%252C%25E8%25B4%25A2%25E7%25BB%258F%252C%25E5%25A8%25B1%25E4%25B9%2590%252C%25E7%25A7%2591%25E6%258A%2580%252C%25E6%2597%25B6%25E5%25B0%259A%252C%25E8%25A7%2586%25E9%25A2%2591%252C%25E8%2588%2586%25E6%2583%2585%252C%25E7%25BD%2591%25E7%25BB%259C%25E8%25AF%2584%25E8%25AE%25BA_%25E7%258E%25AF%25E7%2590%2583%25E7%25BD%2591_%25E7%258E%25AF%25E7%2590%2583%25E6%2597%25B6%25E6%258A%25A5%25E6%2597%2597%25E4%25B8%258B%26ari%3d2%26dbv%3d2%26drs%3d4%26pcs%3d320x270%26pss%3d320x270%26cfv%3d0%26cpl%3d4%26chi%3d1%26cce%3dtrue%26cec%3dUTF-8%26tlm%3d1566454682%26prot%3d2%26rw%3d320%26ltu%3dhttps%253A%252F%252Fboardy.huanqiu.com%252Fsmu0%252Fj.html%26liu%3dhttps%253A%252F%252Fboardy.huanqiu.com%252Fsmu0%252Fh.html%2523ffb3e2aee7fd44dpu4295444aymqacqtaur13226740903040590000t1566471761776(%2525E5%25259B%2525BD%2525E9%252599%252585%2525E6%252596%2525B0%2525E9%252597%2525BB%25252C%2525E5%252585%2525A8%2525E7%252590%252583%2525E5%25258A%2525A8%2525E6%252580%252581%25252C%2525E6%25259C%252580%2525E6%252596%2525B0%2525E6%2525B6%252588%2525E6%252581%2525AF%25252C%2525E5%252586%25259B%2525E4%2525BA%25258B%25252C%2525E5%252586%25259B%2525E6%252583%252585%25252C%2525E5%252585%2525B5%2525E5%252599%2525A8%25252C%2525E7%2525A4%2525BE%2525E4%2525BC%25259A%25252C%2525E8%2525B4%2525A2%2525E7%2525BB%25258F%25252C%2525E5%2525A8%2525B1%2525E4%2525B9%252590%25252C%2525E7%2525A7%252591%2525E6%25258A%252580%25252C%2525E6%252597%2525B6%2525E5%2525B0%25259A%25252C%2525E8%2525A7%252586%2525E9%2525A2%252591%25252C%2525E8%252588%252586%2525E6%252583%252585%25252C%2525E7%2525BD%252591%2525E7%2525BB%25259C%2525E%26ltr%3dhttps%253A%252F%252Fboardy.huanqiu.com%252Fsmu0%252Fj.html%26ecd%3d1%26uc%3d1536x824%26pis%3d320x270%26sr%3d1536x864%26tcn%3d1566471762%26qn%3d12442ec87be1e00e%26tt%3d1566471762092.27.126.127%26lto%3dhttp%253A%252F%252Fwww.yhdm.tv%26ltl%3d2
```

Here is the code

```aidl
public static String recursionDecode(String value) throws UnsupportedEncodingException {
    String tmp;
    while (!(tmp = java.net.URLDecoder.decode(value, "utf-8")).equals(value)) {
        value = tmp;
    }
    return value ;
}
```
Here is the stack trace

```aidl
java.lang.IllegalArgumentException: URLDecoder: Illegal hex characters in escape (%) pattern - For input string: "E&"
	at java.net.URLDecoder.decode(URLDecoder.java:194)
	
```

##  Solution

I found that this url need to be decoded three times using recursionDecode method, after decode three times the url becomes like this:

```aidl
https://pos.baidu.com/hcjm?conwid=320&conhei=270&rdid=4295444&dc=3&exps=110011&psi=d75f47ad5ff815c2e06e410d8173b5f8&di=u4295444&dri=0&dis=3&dai=1&ps=0x0&enu=encoding&dcb=___adblockplus&dtm=HTML_POST&dvi=0.0&dci=-1&dpt=none&tsr=0&tpr=1566471762118&ti=国际新闻,全球动态,最新消息,军事,军情,兵器,社会,财经,娱乐,科技,时尚,视频,舆情,网络评论_环球网_环球时报旗下&ari=2&dbv=2&drs=4&pcs=320x270&pss=320x270&cfv=0&cpl=4&chi=1&cce=true&cec=UTF-8&tlm=1566454682&prot=2&rw=320&ltu=https://boardy.huanqiu.com/smu0/j.html&liu=https://boardy.huanqiu.com/smu0/h.html#ffb3e2aee7fd44dpu4295444aymqacqtaur13226740903040590000t1566471761776(国际新闻,全球动态,最新消息,军事,军情,兵器,社会,财经,娱乐,科技,时尚,视频,舆情,网络%E&ltr=https://boardy.huanqiu.com/smu0/j.html&ecd=1&uc=1536x824&pis=320x270&sr=1536x864&tcn=1566471762&qn=12442ec87be1e00e&tt=1566471762092.27.126.127&lto=http://www.yhdm.tv&ltl=2
```

There is a % between `网络` and `E&` that causes the URLDecoder.decode to be unrecognizable


A solution is to replace % to %25 instead. Something like that:

```aidl
public static String recursionDecode(String value) throws UnsupportedEncodingException {
    String tmp;
    while (!(tmp = java.net.URLDecoder.decode(value, "utf-8").replaceAll("%(?![0-9a-fA-F]{2})", "%25")).equals(value)) {
        value = tmp;
    }
    return value ;
}
```

If you make the above modification,the value of value will be equal to the value of tmp after the third decoding, it will return directly