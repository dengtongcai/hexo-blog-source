---
layout: '[layout]'
title: 为开源软件选择一个合适的开源许可证（Open Source License）
date: 2018/09/15 16:00:12
categories: git license

tags: [git，license]
---

之前知道开源协议这么一回事，比如Apache、Apache2，但是并不知道具体赋予了哪些权力，可能我们使用开源软件的时候只要不是用于商业用途，压根不关心使用的是什么协议。今天在B站看到一个视频讲富文本的，说到一些富文本编辑器使用的开源协议是MIT，可以作为商业使用，突然想了解一下常用的开源协议有哪些，分别有什么权限。

## 常用的开源协议

世界上的开源许可证（Open Source License）大概有上百种，经过[Open Source Initiative](http://www.opensource.org/licenses/alphabetical)组织通过批准的开源协议目前有58种，上文提到的 MIT License 仅仅只是其中的一种而已，而我们常用的开源软件协议大致有[GPL](http://www.gnu.org/licenses/gpl.html)、[BSD](http://en.wikipedia.org/wiki/BSD_licenses)、[MIT](http://en.wikipedia.org/wiki/MIT_License)、[Mozilla](http://www.mozilla.org/MPL/)、[Apache](http://www.apache.org/licenses/LICENSE-2.0)和[LGPL](http://www.gnu.org/copyleft/lesser.html)。我们不必要每个开源协议都了然于心，但是可以了解几个主要的协议的权利和义务。

![1](/1.jpg)

## 详细介绍

### BSD开源协议(original BSD license、FreeBSD license、Original BSD license)

BSD开源协议是一个给于使用者很大自由的协议。

可以自由的使用，修改源代码，也可以将修改后的代码作为开源或者专有软件再发布。当你发布使用了BSD协议的代码，或者以BSD协议代码为基础做二次开发自己的产品时，需要满足三个条件：

- 如果再发布的产品中包含源代码，则在源代码中必须带有原来代码中的BSD协议。
- 如果再发布的只是二进制类库/软件，则需要在类库/软件的文档和版权声明中包含原来代码中的BSD协议。
- 不可以用开源代码的作者/机构名字和原来产品的名字做市场推广。

BSD代码鼓励代码共享，但需要尊重代码作者的著作权。BSD由于允许使用者修改和重新发布代码，也允许使用或在BSD代码上开发商业软件发布和销 售，因此是对商业集成很友好的协议。很多的公司企业在选用开源产品的时候都首选BSD协议，因为可以完全控制这些第三方的代码，在必要的时候可以修改或者 二次开发。

### Apache2.0协议(Apache License， Version 2.0、Apache License， Version1.1、Apache License， Version 1.0)

Apache Licence是著名的非盈利开源组织Apache采用的协议。

该协议和BSD类似，同样鼓励代码共享和尊重原作者的著作权，同样允许代码修改，再发布（作为开源或商业软件）。需要满足的条件也和BSD类似：

- 需要给代码的用户一份Apache Licence

- 如果你修改了代码，需要在被修改的文件中说明。

- 在延伸的代码中（修改和有源代码衍生的代码中）需要带有原来代码中的协议，商标，专利声明和其他原来作者规定需要包含的说明。

- 如果再发布的产品中包含一个Notice文件，则在Notice文件中需要带有Apache Licence。你可以在Notice中增加自己的许可，但不可以表现为对Apache Licence构成更改。

Apache Licence也是对商业应用友好的许可。使用者也可以在需要的时候修改代码来满足需要并作为开源或商业产品发布/销售。

英文原文：http://www.apache.org/licenses/LICENSE-2.0.html

### GPL(GNU General Public License)

我们很熟悉的[Linux](https://www.linuxprobe.com/)就是采用了GPL。GPL协议和BSD，Apache Licence等鼓励代码重用的许可很不一样。

GPL的出发点是代码的开源/免费使用和引用/修改/衍生代码的开源/免费使用，但不允许修改后和衍生的代码做为闭源的商业软件发布和销售。这也就是为什么我们能用免费的各种linux，包括商业公司的linux和linux上各种各样的由个人，组织，以及商业软件公司开发的免费软件了。

GPL协议的主要内容是只要在一个软件中使用(”使用”指类库引用，修改后的代码或者衍生代码)GPL协议的产品，则该软件产品必须也采用GPL协议，既必须也是开源和免费。这就是所谓的”传染性”。GPL协议的产品作为一个单独的产品使用没有任何问题，还可以享受免费的优势。

由于GPL严格要求使用了GPL类库的软件产品必须使用GPL协议，对于使用GPL协议的开源代码，商业软件或者对代码有保密要求的部门就不适合集成/采用作为类库和二次开发的基础。其它细节如再发布的时候需要伴随GPL协议等和BSD/Apache等类似。

### LGPL(GNU Lesser General Public License)

LGPL是GPL的一个为主要为类库使用设计的开源协议。

和GPL要求任何使用/修改/衍生之GPL类库的的软件必须采用GPL协议不同。LGPL允许商业软件通过类库引用(link)方式使用LGPL类库而不需要开源商业软件的代码。这使得采用LGPL协议的开源代码可以被商业软件作为类库引用并发布和销售。

但是如果修改LGPL协议的代码或者衍生，则所有修改的代码，涉及修改部分的额外代码和衍生的代码都必须采用LGPL协议。因此LGPL协议的开源代码很适合作为第三方类库被商业软件引用，但不适合希望以LGPL协议代码为基础，通过修改和衍生的方式做二次开发的商业软件采用。

GPL/LGPL都保障原作者的知识产权，避免有人利用开源代码复制并开发类似的产品。

### MIT（The MIT License）

MIT许可证之名源自麻省理工[学院](https://baike.baidu.com/item/%E5%AD%A6%E9%99%A2/999322)（Massachusetts Institute of Technology, MIT），又称「X条款」（X License）或「X11条款」（X11 License）。

MIT是和BSD一样宽范的许可协议，作者只想保留版权，而无任何其他了限制。也就是说，你必须在你的发行版里包含原许可协议的声明，无论你是以二进制发布的还是以源代码发布的。

- 被许可人权利

被许可人有权利使用、复制、修改、合并、出版发行、散布、再许可和/或贩售软件及软件的副本，及授予被供应人同等权利，惟服从以下义务。

- 被许可人义务

在软件和软件的所有副本中都必须包含以上版权声明和本许可声明。

- 其他重要特性

此许可协议并非属的自由软件许可协议条款，允许在自由及开放源代码软件或非自由软件（proprietary software）所使用。

MIT的内容可依照程序著作权者的需求更改内容。此亦为MIT与[BSD](https://zh.wikipedia.org/wiki/BSD_license)（The BSD license, 3-clause BSD license）本质上不同处。

MIT许可协议可与其他许可协议并存。另外，MIT条款也是自由软件基金会（FSF）所认可的自由软件许可协议条款，与[GPL](https://zh.wikipedia.org/wiki/GPL)兼容。

### MPL( Mozilla Public License  )

MPL协议允许免费重发布、免费修改，但要求修改后的代码版权归软件的发起者 。

这种授权维护了商业软件的利益，它要求基于这种软件的修改无偿贡献版权给该软件。



## 该怎么选择使用哪种开源协议？

下面这个流程图是分析了应该如何选择使用哪一种开源协议。

![2](/2.png)

还有更加完全的英文版

![3](/3.png)



### 参考：

https://zh.wikipedia.org

https://www.oschina.net/news/74999/how-to-choose-a-license

http://www.ha97.com/833.html

