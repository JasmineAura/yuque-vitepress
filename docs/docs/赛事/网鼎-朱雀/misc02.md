拿到一个 jar 包，先用 jd-gui 打开。

发现加密部分。

![](../../../images/df4d0d206fd4e326cc1a20fd5458468f.png)

用的是 AES，然后 key 是 Y4SuperSecretKey。

密文在哪，直接用 jadx-gui 打开 jar 包，然后去交叉引用 newMap 函数。

在 Ll1llllI1LIL.class 中找到一堆密文，试几行就出了。

![](../../../images/9cd6745650c15feb4d1edb560509537d.png)

![](../../../images/4751c4c291e0abb8104d575430bbe3d1.png)

