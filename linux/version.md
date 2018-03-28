# CentOS

CentOS 是一个基于Red Hat Linux 提供的可自由使用源代码的企业级 Linux发行版本。每个版本的 CentOS都会获得十年的支持（通过安全更新方式）。新版本的 CentOS 大约每两年发行一次，而每个版本的 CentOS 会定期（大概每六个月）更新一次，以便支持新的硬件。这样，建立一个安全、低维护、稳定、高预测性、高重复性的 Linux 环境。[1]CentOS是Community Enterprise Operating System的缩写。
CentOS 是 RHEL（Red Hat Enterprise Linux）源代码再编译的产物，而且在 RHEL的基础上修正了不少已知的 Bug ，相对于其他 Linux 发行版，其稳定性值得信赖。RHEL 在发行的时候，有两种方式。一种是二进制的发行方式，另外一种是源代码的发行方式。无论是哪一种发行方式，你都可以免费获得（例如从网上下载），并再次发布。但如果你使用了他们的在线升级（包括补丁）或咨询服务，就必须要付费

1[3]．可以把CentOS理解为Red Hat AS系列！它完全就是对Red Hat AS进行改进后发布的！各种操作、使用和RED HAT没有区别！
2．CentOS完全免费，不存在RED HAT AS4需要序列号的问题。
3．CentOS独有的yum命令支持在线升级，可以即时更新系统，不像RED HAT那样需要花钱购买支持服务！
4．CentOS修正了许多RED HAT AS的BUG！
5．CentOS版本说明：CentOS3.1 等同于 RED HAT AS3 Update1 CentOS3.4 等同于 RED HAT AS3 Update4 CentOS4.0 等同于 RED HAT AS4
与 RHEL的关系
RHEL 在发行的时候，有两种方式。一种是二进制的发行方式，另外一种是源代码的发行方式。无论是哪一种发行方式，你都可以免费获得（例如从网上下载），并再次发布。但如果你使用了他们的在线升级（包括补丁）或咨询服务，就必须要付费。RHEL 一直都提供源代码的发行方式，CentOS 就是将 RHEL 发行的源代码重新编译一次，形成一个可使用的二进制版本。由于 LINUX 的源代码是 GNU，所以从获得 RHEL 的源代码到编译成新的二进制，都是合法。只是 REDHAT 是商标，所以必须在新的发行版里将 REDHAT 的商标去掉。REDHAT 对这种发行版的态度是："我们其实并不反对这种发行版，真正向我们付费的用户，他们重视的并不是系统本身，而是我们所提供的商业服务。" 所以，CentOS 可以得到 RHEL 的所有功能，甚至是更好的软件。但 CentOS 并不向用户提供商业支持，当然也不负上任何商业责任。如果你要将你的 RHEL 转到 CentOS 上，因为你不希望为 RHEL 升级而付费。当然，你必须有丰富 linux 使用经验，因此 RHEL 的商业技术支持对你来说并不重要。但如果你是单纯的业务型企业，那么还是建议你选购 RHEL 软件并购买相应服务。这样可以节省你的 IT 管理费用，并可得到专业服务。一句话，选用 CentOS 还是 RHEL，取决于你所在公司是否拥有相应的技术力量。

# Mandriva

Mandriva原名Mandrake，最早由Ga?l Duval创建并在1998年7月发布。记得前两年国内刚开始普及Linux时，Mandrake非常流行。说起Mandrake的历史，其实最早 Mandrake的开发者是基于Redhat进行开发的。Redhat默认采用GNOME桌面系统，而Mandrake将之改为KDE。而由于当时的 Linux普遍比较难安装，不适合第一次接触Linux的新手，所以Mandrake还简化了安装系统。我想这也是当时Mandrake在国内如此红火的 原因之一。Mandrake在易用性方面的确是下了不少功夫，包括默认情况下的硬件检测等。

Mandrake的开发完全透明化，包括“cooker”。当系统有了新的测试版本后，便可以在cooker上找到。之前Mandrake的新版本的发布速度很快，但从9.0之后便开始减缓。估计是希望能够延长版本的生命力以确保稳定和安全性。

优点：友好的操作界面，图形配置工具，庞大的社区技术支持，NTFS分区大小变更

缺点：部分版本bug较多，最新版本只先发布给Mandrake俱乐部的成员

软件包管理系统：urpmi (RPM)

免费下载：FTP即时发布下载，ISO在版本发布后数星期内提供

官方主页：http://www.mandrivalinux.com/

# Red Hat

国内，乃至是全世界的Linux用户所最熟悉、最耳闻能 详的发行版想必就是Red Hat了。Red Hat最早由Bob Young和Marc Ewing在1995年创建。而公司在最近才开始真正步入盈利时代，归功于收费的Red Hat Enterprise Linux(RHEL，Red Hat的企业版)。而正统的Red Hat版本早已停止技术支持，最后一版是Red Hat 9.0。于是，目前Red Hat分为两个系列：由Red Hat公司提供收费技术支持和更新的Red Hat Enterprise Linux，以及由社区开发的免费的Fedora Core。Fedora Core 1发布于2003年年末，而FC的定位便是桌面用户。FC提供了最新的软件包，同时，它的版本更新周期也非常短，仅六个月。目前最新版本为FC 3，而FC4也预定将于今年6月发布。这也是为什么服务器上一般不推荐采用Fedora Core。

适用于服务器的版本是Red Hat Enterprise Linux，而由于这是个收费的操作系统。于是，国内外许多企业或空间商选择CentOS。CentOS可以算是RHEL的克隆版，但它最大的好处是免费！菜鸟油目前的服务器便采用的CentOS 3.4。

优点：拥有数量庞大的用户，优秀的社区技术支持，许多创新

缺点：免费版(Fedora Core)版本生命周期太短，多媒体支持不佳

软件包管理系统：up2date (RPM)， YUM (RPM)

免费下载：是

官方主页：http://www.redhat.com/

# SUSE

SUSE是德国最著名的Linux发行版，在全世界范围中也享有较高的声誉。SUSE自主开发的软件包管理系统YaST也大受好评。SUSE于2003年年末被Novell收购。

SUSE之后的发布显得比较混乱，比如9.0版本是收费的，而10.0版本(也许由于各种压力)又免费发布。这使得一部分用户感到困惑，也转而使用其它发行版本。但是，瑕不掩瑜，SUSE仍然是一个非常专业、优秀的发行版。

优点：专业，易用的YaST软件包管理系统

缺点：FTP发布通常要比零售版晚1~3个月

软件包管理系统：YaST (RPM)， 第三方APT (RPM) 软件库(repository)

免费下载：取决于版本

官方主页：http://www.suse.com/

# Debian GNU/Linux

Debian是菜鸟油服务器之前所采用的操作系统。 Debian最早由Ian Murdock于1993年创建。可以算是迄今为止，最遵循GNU规范的Linux系统。Debian系统分为三个版本分支 (branch)：stable, testing 和 unstable。截至2005年5月，这三个版本分支分别对应的具体版本为：Woody, Sarge 和 Sid。其中，unstable为最新的测试版本，其中包括最新的软件包，但是也有相对较多的bug，适合桌面用户。testing的版本都经过 unstable中的测试，相对较为稳定，也支持了不少新技术(比如SMP等)。而Woody一般只用于服务器，上面的软件包大部分都比较过时，但是稳定 和安全性都非常的高。菜鸟油之前所采用的是Debian Sarge。

Debian

为何有如此多的用户痴迷于Debian呢(包括笔者在 内)？apt-get / dpkg是原因之一。dpkg是Debian系列特有的软件包管理工具，它被誉为所有Linux软件包管理工具(比如RPM)最强大的！配合apt- get，在Debian上安装、升级、删除和管理软件变得异常容易。许多Debian的用户都开玩笑的说，Debian将他们养懒了，因为只要简单得敲一 下”apt-get upgrade && apt-get update”，机器上所有的软件就会自动更新了……

优点：遵循GNU规范，100%免费，优秀的网络和社区资源，强大的apt-get

缺点：安装相对不易，stable分支的软件极度过时

软件包管理系统：APT (DEB)

免费下载：是

官方主页：http://www.debian.org/

# Ubuntu

简单而言，Ubuntu就是一个拥有Debian所有的 优点，以及自己所加强的优点的近乎完美的Linux操作系统。:) Ubuntu是一个相对较新的发行版，但是，它的出现可能改变了许多潜在用户对Linux的看法。也许，从前人们会认为Linux难以安装、难以使用，但 是，Ubuntu出现后，这些都成为了历史。Ubuntu基于Debian Sid，所以这也就是笔者所说的，Ubuntu拥有Debian的所有优点，包括apt-get。然而，不仅如此而已，Ubuntu默认采用的GNOME 桌面系统也将Ubuntu的界面装饰的简易而不失华丽。当然，如果你是一个KDE的拥护者的话，Kubuntu同样适合你！

Ubuntu的安装非常的人性化，只要按照提示一步一步 进行，安装和Windows同样简便！并且，Ubuntu被誉为对硬件支持最好最全面的Linux发行版之一，许多在其他发行版上无法使用，或者默认配置 时无法使用的硬件，在Ubuntu上轻松搞定。并且，Ubuntu采用自行加强的内核(kernel)，安全性方面更上一层楼。并且，Ubuntu默认不 能直接root登陆，必须从第一个创建的用户通过su或sudo来获取root权限(这也许不太方便，但无疑增加了安全性，避免用户由于粗心而损坏系 统)。Ubuntu的版本周期为六个月，弥补了Debian更新缓慢的不足。

优点：人气颇高的论坛提供优秀的资源和技术支持，固定的版本更新周期和技术支持，可从Debian Woody直接升级

缺点：还未建立成熟的商业模式

软件包管理系统：APT (DEB)