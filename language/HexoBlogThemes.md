# Hexo博客框架-切换主题、删除文章

## 一. 前言

搭建好hexo博客之后，默认情况下，所有人的主题都是landscape，意味着大家的博客样子都是一样的，
如下图所示（图为百度经验博主angelen文章的截图）：

![angelen's blog](/assets/HexoBlogThemes/HBT1.png)

那么为了让自己的博客更加有个性一些，一件比较有意思的事情就是如何更换自己的主题（themes），
也有些人称之为皮肤或者模板，都是一个意思——就是你现在博客的样子。

同时，打开自己的主页之后首先映入眼帘的就是hexo默认的一篇helloworld的博客。在我们正式写作之前，
这篇默认的helloworld博客仅仅是一个样板，让我知道自己的博客是可以使用的，但是真正当我们打算开始用我们的博客之后，
这篇helloworld文章是没有任何意义的，我们需要删掉它。所以我们也需要学会如何删除博客。

本文就主要讲一下如何修改自己的博客主题，以及如何删除文章这两部分内容。

## 二. 如何修改主题

- 下载主题`git clone git@github.com:litten/hexo-theme-yilia.git`
- 将下载好的主题包复制到Hexo项目themes文件夹中
- 配置`_config.yml`文件中`theme`为对应主题名称
- 执行`hexo g`和`hexo d`
- 大功告成

hexo有很多主题，每个人可以选择自己喜欢的主题来应用，也可以自己设计主题并且上传形成公共主题供大家下载。

如果是自己设计主题的话，会稍微麻烦一些，需要自己配置很多文件，并且编写css以及js代码等。
关于如何编辑自己的主题并且发布到公共主题这方面的内容，可以参考hexo官方文档中themes部分的内容，
其中`publishing`的内容就是如何发布自己主题的流程。

- 传递门：[HEXO官网 Themes版块](https://hexo.io/docs/themes.html)

自己制作主题并且发布供所有人共享的情况稍微复杂一些，这里不做详细说明，感兴趣的同学看上面传递门中的内容即可。
这里主要讲解下，如何把主题修改为公共库中的主题，把自己的博客改造成自己喜欢的样子。

我们知道hexo官方共享的themes有很多，可以在[hexo themes collection](https://github.com/hexojs/hexo/wiki/Themes)中查看。

打开之后看到的这些themes都是可以供我们直接应用和安装的主题。那么如何才能应用某一种主题呢？

主要分为下面几个步骤：

- 下载主题（themes）；
- 修改主目录文件夹中的_config.yml文件的theme配置；
- 执行hexo generta => hexo deploy将主题应用到自己最新的博客上面。

我们以Tinnypp这个主题为例来讲解具体如何应用该主题。

首先，点击Tinnypp这个链接，打开对应的theme主页，如下图：

![点击Tinnypp](/assets/HexoBlogThemes/HBT2.png)

进入到相应的页面之后，我们可以先在About部分中查看这个主题的demo，看是否是我们想要的样子。
假设查看之后，我们很喜欢，决定应用这个主题。然后再到Installation部分查看如何安装，其实上面已经很明确地告诉了我们如何安装这个主题。

![如何安装Tinnypp主题](/assets/HexoBlogThemes/HBT3.png)

首先进入到主目录文件夹下的themes文件夹，你会发现有一个landscape文件夹，这个文件夹就是landscape这个主题的安装包，
也就是我们现在博客的主题。然后在themes文件夹下，执行上面所说的git clone git@github…的命令，如下图

![git clone](/assets/HexoBlogThemes/HBT4.png)

等待下载过程完成后，进入到主目录下，修改_config.yml文件中themes的值。

![修改_config.yml](/assets/HexoBlogThemes/HBT5.png)

进行完以上步骤之后，依次执行`hexo g`，`hexo d`，然后再打开自己的主页看看，发现页面的主题已经变成了Tinnypp。
有些网上的文章说需要等待十多分钟，自己对主题所进行的设置才会呈现，其实根据我的经验，部署命令和主页的效果基本上是同步的，
也就是说当你部署完成之后，立即打开自己的主页其实就会看到新的主题效果。如果大家没有看到，可以稍微等待一下然后再查看，
如果等待很久发现还是没有更换主题，那么请重新按照上面的步骤检查一遍，一定是哪里出了问题。

以上就是整个更改主题的过程。我们可以根据自己的个人兴趣爱好在上面提供的
[hexo themes collection](https://github.com/hexojs/hexo/wiki/Themes)中选择任何自己喜欢的主题进行安装。
先预览，再安装，但是主题太多了，一下子不知道该如何选择了，眼睛都挑花了。因为官方themes实在是太多了，
所以这里我推荐几个还不错的themes供大家参考。

- [Tinnypp](https://github.com/levonlin/Tinnypp)

写这篇文章的时候，其实我也没看过几个主题，所以就暂时先放一个占位符，等以后自己应用了其他主题之后，
再把自己觉得不错的主题慢慢增加上来。希望大家可以持续关注我的博客，会有不断的迭代更新。

讲完了如何修改主题，我们该讲解如何删除文章了。

## 三. 如何删除文章

删除文章的过程一样也很简单，先删除本地文件，然后通过生成和部署命令进而将远程仓库中的文件也一并删除。
具体来说，以最开始默认形成的helloworld.md这篇文章为例。

首先进入到source / _post 文件夹中，找到helloworld.md文件，在本地直接执行删除。
然后依次执行`hexo g`，`hexo d`，再去主页查看你就会发现你的博客上面已经空空如也了，这就是如何删除文章的方法。

## 四. 总结

通过上面如何更改themes以及如何删除文章的操作，我们可以了解到整个主目录下themes文件夹和source这两个子文件夹的意义和作用。

themes文件夹下就是我们的主题安装包的内容，你现在打开之后会发现有Landscape和Tinnypp这两个主题的安装包。

而source下面的_post文件夹就是我们所发表的文章的原始文件，例如我们刚才删除的Helloworld.md文件就是我们要post到网上的文章。
这里我顺便把整个主目录下的其他子文件夹的内容也简单说一下。

![主目录下的内容](/assets/HexoBlogThemes/HBT6.png)

1. node_modules文件夹中的内容是整个hexo的依赖包，这个一般情况下不需要去做任何增删改。

2. public文件夹其实是当我们执行hexo generate命令之后由hexo根据我们的主题以及文章内容生成的一个文件。
打开之后可以看到对应的时间我们写的博客内容，例如我今天的博客就是pubulic / 2016 / 05 / 01 / 如何更换hexo主题、删除文章.md这篇文章。
你可以新建一篇文章来看看pubulic文件夹下面的变化，不过因为本文还没有讲解如何生成新的文章，所以你先不用着急，
我会在后面的博客中慢慢讲解更多关于hexo如何应用的文章。

3. scaffolds其实就是脚手架的意思，有些人也称之为模板，其实就是我们新生成一篇文章时从哪个模板来生成。
这里面默认只有三个模板，draft, page, post，我们生成文章是可以指定采用哪个模板。

4. source和themes子文件夹不再讲解，上面已经解释过了。
_config.yml文件，这个其实是我们整个主目录的配置，你会发现你把主题从landscape切换到Tinnypp时，
除了下载主题包之外就是在这个文件里去配置自己的主题，只需要更改少量的键值对内容就可以对整个主目录和网页进行变更了。

5. db.json，没有仔细研究，后期会更新这部分的内容。

6. package.json，没有仔细研究，后期会更新这部分的内容。

以上就是本文所要讲解的全部内容。

## 五. 其他

我发现自己在网上找的很多文章有两种情况，一种是系统性的将hexo进行完整的讲解，另外一种是只讲解其中的一个部分。
其实对于初学者来说，这两类文章都是比较头疼的事情，如果是看全部讲解的内容的话，其实很多时候找不到重点，
没有一个循序渐进的过程，尤其是需要给他一个点，让他可以实现，进而增加他继续将这件事情进行下去的成就感。
而如果只是讲解其中一小部分的内容的话，又会觉得自己还有很多疑问没有得到解答。那么其实最好的方式就是把系统性的Hexo攻略，
按照顺序依次分成多篇博客文章进行讲解，这样会让初学者有个循序渐进的过程，前期可以抓住重点，抓住骨干性的内容学习，
后期自然就可以学会自己去研究其中的血肉和皮肤了，甚至可能不需要再详细讲解其中的血肉部分，他就会自己去研究官方文档得到答案。
其实基础打好，攻略看到后面会越看越顺也越快越熟悉。所以我才会把这篇关于hexo的介绍分成这么多篇博客来与大家共享。
希望大家可以继续跟进我的博客，后面会有更精彩的内容与大家分享。