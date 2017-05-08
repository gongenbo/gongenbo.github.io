---
layout: post
title: PHP汉字转拼音更准确的解决方案
excerpt: 到目前为止这可能是最准确的一个解决方案了，大GitHub上各种找，没有一个好用的，大部分都只是汉字转拼音,所以我造了这个轮子...
---

到目前为止这可能是最准确的一个解决方案了，大GitHub上各种找，没有一个好用的，大部分都只是汉字转拼音，所以包含多音字的结果基本都错误。当然也有基于词典的转换工具，不过还是解决不了词库不全造成的多音字问题（当然，完全解决是不太可能的，或者词库会无比的大）。比如`康熙来了` 大部分工具的试用结果是：kang xi lai liao.

基于上面的原因，我造了下面这个轮子。特点如下:

- 基于[CC-CEDICT](http://cc-cedict.org/wiki/).词典，解决大部分词汇问题；
- 添加补充词典，解决CC-CEDICT不全或者不准确的问题；
- 添加词频表，根据使用频率再一步提高多音字的准确度；

[Pinyin](http://overtrue.me/pinyin)
======

主页：[http://overtrue.me/pinyin](http://overtrue.me/pinyin)

基于CC-CEDICT词典的中文转拼音工具, 更准确的汉字转拼音解决方案。 [CC-CEDICT](http://cc-cedict.org/wiki/).

```php?start_inline=1
use \Overtrue\Pinyin;

echo Pinyin::pinyin('带着希望去旅行，比到达终点更美好');
// dài zhe xī wàng qù lǔ xíng bǐ dào dá zhōng diǎn gèng měi hǎo

//多音字
// 了
Pinyin::pinyin('了然'); // liǎo rán
Pinyin::pinyin('来了'); // lái le

// 还
Pinyin::pinyin('还有'); // hái yǒu
Pinyin::pinyin('交还'); // jiāo huán

// 什
Pinyin::pinyin('什么'); // shén me
Pinyin::pinyin('什锦'); // shí jǐn

// 便
Pinyin::pinyin('便当'); // biàn dāng
Pinyin::pinyin('便宜'); // pián yí

// 剥
Pinyin::pinyin('剥皮'); // bāo pí
Pinyin::pinyin('剥皮器'); // bō pí qì

// 不
Pinyin::pinyin('赔不是'); // péi bú shi
Pinyin::pinyin('跑了和尚，跑不了庙'); // pǎo le hé shàng , pǎo bù liǎo miào

// 降
Pinyin::pinyin('降温'); // jiàng wēn
Pinyin::pinyin('投降'); // tóu xiáng

// 都
Pinyin::pinyin('首都'); // shǒu dū
Pinyin::pinyin('都什么年代了'); // dōu shén me nián dài le

// 乐
Pinyin::pinyin('快乐'); // kuài lè
Pinyin::pinyin('音乐'); // yīn yuè

// 长
Pinyin::pinyin('成长'); // chéng zhǎng
Pinyin::pinyin('长江'); // cháng jiāng

// 难
Pinyin::pinyin('难民'); // nàn mín
Pinyin::pinyin('难过'); // nán guò
...

```


# 安装
1. 使用 Composer 安装:

```shell
composer require overtrue/pinyin >=1.4
```

或者在你的项目composer.json加入：

```javascript
{
    "require": {
        "overtrue/pinyin": ">=1.4"
    }
}
```

2. 直接下载文件 `src/Overtrue/Pinyin.php` 引入到项目中。


# 使用

```php?start_inline=1
<?php
use \Overtrue\Pinyin;

//获取拼音
echo Pinyin::pinyin('带着希望去旅行，比到达终点更美好');
//或者: Overtrue\pinyin($string);
// dài zhe xī wàng qù lǔ xíng bǐ dào dá zhōng diǎn gèng měi hǎo

//获取首字母
echo Pinyin::letter('带着希望去旅行，比到达终点更美好');
// D Z X W Q L X B D D Z D G M H

```

## 设置

- `delimiter` 分隔符，默认为一个空格 ' '；
- `traditional` 繁体
- `accent` 是否输出音调；
- `letter` 只输出首字母，或者直接使用`Pinyin::letter($string)`;
- `only_chinese` 只保留$string中中文部分。

* 全局设置：*  `Pinyin::set('delimiter', '-');`

* 临时设置：*  `Pinyin::pinyin($word, $settings)` 在调用的方法后传参

example:

```php?start_inline=1

Pinyin::set('delimiter', '-');//全局
echo Pinyin::pinyin('带着希望去旅行，比到达终点更美好');

// dài-zhe-xī-wàng-qù-lǔ-xíng-bǐ-dào-dá-zhōng-diǎn-gèng-měi-hǎo
```
```php?start_inline=1

$setting = [
            'delimiter' => '-',
            'accent' => false,
           ];

echo Pinyin::pinyin('带着希望去旅行，比到达终点更美好', $setting);//这里的setting只是临时修改，并非全局设置

// dai-zhe-xi-wang-qu-lu-xing-bi-dao-da-zhong-dian-geng-mei-hao
```

```php?start_inline=1
Pinyin::set('accent', false);
echo Pinyin::pinyin('带着希望去旅行，比到达终点更美好');

// dai zhe xi wang qu lu xing bi dao da zhong dian geng mei hao
```

# Contribution
欢迎提意见及完善补充词库 `src/Overtrue/data/additional.php`! :kiss:

# 参考
- [CC-CEDICT](http://cc-cedict.org/wiki/)
- [現代漢語語音語料庫](http://mmc.sinica.edu.tw/intro_c_01.html)
- [汉典](http://www.zdic.net/)

# License

MIT
