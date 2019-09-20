* [ReleaseZip2Jcenter](#releasezip2jcenter)
  * [1 私有项目发布到 jCenter public 仓库](#1-私有项目发布到-jcenter-public-仓库)
     * [1.1 背景](#11-背景)
     * [1.2 思路](#12-思路)
     * [1.3 具体步骤](#13-具体步骤)
  * [2 将三方 aar 发布到 jCenter public 仓库](#2-将三方-aar-发布到-jcenter-public-仓库)
     * [2.1 背景](#21-背景)
     * [2.2 思路](#22-思路)
     * [2.3 具体步骤](#23-具体步骤)

# ReleaseZip2Jcenter

上传项目到 jCenter 并发布至 public 仓库

## 1 私有项目发布到 jCenter public 仓库

### 1.1 背景
有私有项目想上传到 jCenter 并 public，一直被拒审。

### 1.2 思路
创建一个新仓库并加些源码，将 module 名改为私有项目名一致，
使用 release-aar 将 module 打出 aar，手动将此 aar 发布到 jCenter 审核通过后再用脚本发布目标私有项目，
这样就避免了审核，以后就可以通过脚本更新私有项目到 public jCenter 了。

### 1.3 具体步骤

以发布 pubnative 为例

1. 在 jCenter 创建仓库，库名为 pubnative，版本控制为 https://github.com/zmer007/ReleaseZip2Jcenter
2. 配置 pubnative gradle 执行 ```./gradlew pubnative:generateRelease``` 获取 zip 包
3. 将 zip 上传到步骤 1 创建的仓库，点击 add Jcenter

首次发版后，后续就可以使用脚本了


## 2 将三方 aar 发布到 jCenter public 仓库

### 2.1 背景
接入三方 SDK 时，对方提供的是 aar（记为 A.aar） 没有提供远程依赖的方法，我们
发布的 SDK 集成了 A.aar，但是想将自己的 SDK 发布到 jCenter public 仓库。如果将 
A.aar 添加到 lib 目录下，是不会将 A.aar 一块上传到 jCenter 的，其它开发者接入我们的
SDK 就必须手动将 A.aar 添加到他们 lib 里，然后再引入。

### 2.2 思路
在自己 jCenter 仓库下创建一个 A.aar 同名项目，将 A.aar 上传到此项目中

### 2.3 具体步骤

以 GDT.aar 为例

1. 在 jCenter 创建仓库，库名为 GDT, 版本控制为  https://github.com/zmer007/ReleaseZip2Jcenter
2. 配置 gdt gradle 执行 ```./gradlew pubnative:generateRelease``` 获取 zip 包
3. 解压 zip 包找到包中的 aar 及 pom 文件，将 A.aar 重命名后替换包中的 aar，如果 A.aar 有其它依赖就将其添加到 pom 文件中
4. 修改 aar、pom 文件的 md5 值及 sha1 值，在命令行中执行 ```md5 filePath``` 获取 md5 值，执行 ```shasum filePath``` 获取 sha1 值
5. 将解压的目录重新打成 zip 包，然后上报到步骤 1 创建的仓库