## <font style="color:#D22D8D;">Trace</font>
给了一个图片，010 发现图片后面有一大堆 base64，解一下。

![](../../../../images/6f153ee23fc36781c95da1fa493eaf73.png)

解出来是个 rar 压缩包，发现有密码。

ARCHPR 爆不了，因为这是 rar5，只能使用 john 搭配 hashcat 进行爆破。

爆出来以后解出来一张怪怪的图片。

![](../../../../images/9735ef14e279a6048d65a30e404c74bc.png)

不过很好修，用 windows 自带的画图工具，自由选择+透明选择根据轮廓拼起来即可。

![](../../../../images/ef8de0ff6dcef316a75275e1eb8443a8.png)

