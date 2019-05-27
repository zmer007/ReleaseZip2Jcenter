# ReleaseZip2Jcenter

背景：有私有项目想上传到 jcenter 并 public，一直被拒审。

思路：创建一个新仓库并加些源码，将 module 名改为私有项目名一致，使用 release-aar 将 module 打出 aar，手动将此 aar 发布到 jcenter 审核通过后再用脚本发布目标私有项目，这样就避免了审核，以后就可以通过脚本更新私有项目到 public jcenter 了。

具体步骤：（以发布 pubnative 为例）

1. 在 jcenter 创建仓库，库名为 pubnative，版本控制为 https://github.com/zmer007/ReleaseZip2Jcenter
2. 配置 pubnative gradle 执行 ./gradlew pubnative:generateRelease 获取 zip 包
3. 将 zip 上传到步骤 1 创建的仓库，点击 add Jcenter

首次发版后，后续就可以使用脚本了
