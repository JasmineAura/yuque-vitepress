# <font style="color:#601BDE;">C</font><font style="color:#AE146E;">rypto</font>
## <font style="color:#5C8D07;">古典密码</font>
```plain
你害怕吃日本的海鲜吗, 哎，怕的是水污染哦。
```

句子首字母是 nhpcrbdhxm   apdsswro ，最后几个刚好是 password 调换一下顺序，本来的password 依次对应 1~8。

```plain
scuctf{21834576} 
```

## <font style="color:#5C8D07;">sign</font>
```python
from Ctypto.Util.number import bytes_to_long
flag = b'************'
flag = bytes_to_long(flag)
print(flag**2)


```

开根号就行了。

```python
from Ctypto.Util.number import long_to_bytes
from gmpy2 import iroot
m2=634224651564898754539998157442756001083664116199826003787455223670065793192082549055454281560984604909069350043903900173786578435523771936648969
flag = long_to_bytes(gmpy2.iroot(m2,2)[0])
print(flag)


```



## <font style="color:#5C8D07;">Funny</font>
![image](../../../images/4ca7b806922b40e89ef85ee40655a1ec.svg)

<font style="color:#000000;">可以看出 </font><font style="color:#000000;">r</font><font style="color:#000000;"> 的值不大，故可通过暴力枚举将其变为已知量。</font>

<font style="color:#000000;">由 </font>![image](../../../images/70a6ef79981594cf2e0506a1073dacb0.svg)<font style="color:#000000;"> 有：</font>![image](../../../images/a9978a4e90a8822a8fbb455919ea6801.svg)<font style="color:#000000;">。</font>

<font style="color:#000000;">由 </font>![image](../../../images/30622b2a4539f5bfe6c0a146d73e7e14.svg)<font style="color:#000000;"> 与 </font>![image](../../../images/fc7e1d645984ce1fcb95cc33d2dca9fa.svg)<font style="color:#000000;"> 可得 </font>![image](../../../images/6381c055b33cc38460f27d6507f7ea17.svg)<font style="color:#000000;">。</font>

<font style="color:#000000;">由 </font>![image](../../../images/ab960f39099c968dab0481fb457bf01b.svg)<font style="color:#000000;"> 可得 </font>![image](../../../images/035abe3ead83a339b9baecbb82697361.svg)<font style="color:#000000;"> 的值，进而得到 </font>![image](../../../images/5371e468e9e664aa704657d0ce5f77df.svg)<font style="color:#000000;"> 的值。</font>

<font style="color:#000000;">在本处枚举的情况下，若计算出 </font>![image](../../../images/035abe3ead83a339b9baecbb82697361.svg)<font style="color:#000000;"> 的值为整数，则可认为对应的 </font>![image](../../../images/b56c95cde5b66fdc59588bcfd0d7e43a.svg)<font style="color:#000000;"> 是正确的值。</font>

<font style="color:#000000;">脚本如下：</font>

```python
from gmpy2 import *
f1 = -17337117227294603081644913925715986304757691186817314168716070675168728237679560788649529089548008347749038616949165538274455486179917458595151081640403539751423616759911713840638803490704488888721766198501634924947022411482611118431514836628403198910259213109382128421336826918304936929700914718767612296481855186054998920368855241548606650200550250310760914787293267642488292723273789390824832638679706543869841856637405754914855770290337227026645086808969790740018490395742739684465737948586702801081011607546494377177946129929765696444973369402105823853124703107856231998873101691105985504867357684501470651676994
f2 = 2581942792046579713395674014031211416173666863245901819258268603261755307243320667442197467723761019147008091786796111430664620522079713416215205447517403606615578802785520575916277968997248407919640482821112495366403204438080109398559116192012622524041467314942629801675105262865770389436746211454630897001963684781627802184497065030574782227270688908311334935704323921711566369173579
f3 = 44763444859879884038708658157008071880193344021551817387637745646314918339097045709994070424282107069715320569002772481919510646881596998094820773585351446686773947691955953853080332090535293214509733516261956478846204995740314555061149760847579734281394837479231038061498909195978778319116247773847103941535002072810562456449510361013529041463649306865813540918989501574478338995331113256493656141606708985410790426190958080972964284821663546051159974442013349466348868665365554757942129730848231929177240461721610495864022353897821521000498197455840419046869625025867297390870700359575928362411304198406562760301775578261436870821464216607554428437454146143979543436426533991556100659579987696593675091495042256176364675835021975637786279647637390302897317143393283841691008109978628103890163229468157845393583463465476351193506396823483251415221884952122736937783458347186979788716669539126989667368466070307821665482034597718884079590052885746185215558146479388778532643628478292807033377728626986
n = (f3+0x401)//f2
for r in range(-0x401,0x402):
    p_add_q = f1+n-r
    buffer = (p_add_q*p_add_q)-4*n
    val1,val2 = gmpy2.iroot(buffer,2)
    if(val2 != 1):
        continue
    p = p_add_q+val1
    q = p_add_q-p
    print("%d,%d,%d"%(r,p,q))

    
```

![image](../../../images/5e193daa71d1a9bf7fa3804abd9e6d87.svg)

<font style="color:#000000;">假定 </font>![image](../../../images/cd886ef3886ac3441b05ad03fdbd3a1a.svg)<font style="color:#000000;">，故认为有 </font>![image](../../../images/1619ebab2dbb0ba6fb2bc9dd44413628.svg)<font style="color:#000000;">。</font>

<font style="color:#000000;">脚本如下：</font>

```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import invert
f4 = 3208444748775539679899380105832032233046107978893625201975742354386860817426404645710580786436813251982166398072123607126977075577443383253105784221005811832541925472428712962137721775112788798165406344192849479715687308430038258994227891625783250837464249165684689659268781499736581159743935510110138755706049390867510171680853806801742557749628401098886471260368642083089738695645932070375224567225560860488495595243411181755476610570353182439447822323563265416597218638205622062808123779413183491848361433630614746423079089799312966780304142986457944759368616398610715556048774076274844317753805775880116020089014
e = 65537
p = 153427103974418719544419406258493511020150443477637702597746483814150954755074791792941629143424323372952861739360195469122679380312579050052908701206923224982233720758410695499610865878670625104586143824811001310486280487058427856618172868206549381881516940781212037789957049085298008493488349137836616424991
q = 112999051524724493984540652706668474289153459570577286302519555588693406643094878611330847883900018864558900635994343923025526015488493881949526130631958256095295072723172452685279504037382956062196189297502035275834736568268073519915186472071481082944870471033384862155253352503573512836280436471333887547199
n = p*q
phi = (p-1)*(q-1)
d = invert(e,phi)
m = pow(f4,d,n)
print(m,"\n",long_to_bytes(m))


```

<font style="color:#000000;">由 </font>![image](../../../images/4d0fe6da12c238481756844214c449d8.svg)<font style="color:#000000;"> 有：</font>![image](../../../images/37ae2144c5f2c7dc326a35182c755dcd.svg)<font style="color:#000000;">。由上，</font>![image](../../../images/746450268a7b5b62d72e59129d387143.svg)<font style="color:#000000;"> 已知，故可求 </font>![image](../../../images/b8d76ade3926732b6f05bd1166df3446.svg)<font style="color:#000000;">。</font>

```plain
b'fake:scuctf{777244834953575180597136747909171714123072108442216566907495718016920431857250692477}'
```

<font style="color:#000000;">再 long_to_bytes 一次。</font>

```plain
flag:scuctf{0x401_Is_The_Best_Team}
```



## <font style="color:#5C8D07;">Sh0wTime</font>
<font style="color:#000000;">分析代码可知，本处涉及 </font>**<font style="color:#000000;">ECDSA 数字签名算法</font>**<font style="color:#000000;">。</font>

<font style="color:#000000;">本处源代码给出了三个可供选择的操作  </font>`<font style="color:#000000;">sign_time</font>`<font style="color:#000000;">,</font>`<font style="color:#000000;">verify</font>`<font style="color:#000000;"> 与 </font>`<font style="color:#000000;">challenge</font>`<font style="color:#000000;">。每次连接靶机后只有两次操作机会。其中：</font>

+ `<font style="color:#000000;">sign_time</font>`<font style="color:#000000;"> 的作用是以时间生成明文字符串、通过固定的密钥进行加密，返回明文与密文。</font>
+ `<font style="color:#000000;">verify</font>`<font style="color:#000000;"> 的作用是通过输入明文与对应密文检查二者是否对应，若对应则输出 </font>`<font style="color:#000000;">hint</font>`<font style="color:#000000;">。</font>
+ `<font style="color:#000000;">challenge</font>`<font style="color:#000000;"> 的作用是输入求出的 </font>![image](../../../images/26d9e6b17dd11dbc59bebbce2d901073.svg)<font style="color:#000000;">，若值正确则返回 </font>`<font style="color:#000000;">flag</font>`<font style="color:#000000;">。</font>

<font style="color:#000000;">故本题求 </font>`<font style="color:#000000;">flag</font>`<font style="color:#000000;"> 的问题被转换为求 </font>![image](../../../images/26d9e6b17dd11dbc59bebbce2d901073.svg)<font style="color:#000000;">。</font>

<font style="color:#000000;">观察  </font>`<font style="color:#000000;">sign_time</font>`<font style="color:#000000;"> 中明文的生成格式，可看出：同一秒内生成的明文相同，经过同一加密过程所得到的密文也相同。</font>

<font style="color:#000000;">由题，同一秒内，每次连接后先做 </font>`<font style="color:#000000;">sign_time</font>`<font style="color:#000000;"> 操作 再做 </font>`<font style="color:#000000;">verify</font>`<font style="color:#000000;"> 操作可得 hint。</font>

<font style="color:#000000;">通过 </font>`<font style="color:#000000;">pwntools</font>`<font style="color:#000000;"> 工具，我们可以将连接靶机的操作自动化处理。</font>

<font style="color:#000000;">最终得到 </font>`<font style="color:#000000;">hint:maybe you can try to take care of the time</font>`

<font style="color:#000000;">根据 </font>[<font style="color:#000000;">此处</font>](https://hasegawaazusa.github.io/ecdsa-note.html)<font style="color:#000000;"> 的相关内容，可得：</font>![image](../../../images/713a5af57186489545897a5d6188ed19.svg)<font style="color:#000000;">。而在秒数为1时调用 </font>`<font style="color:#000000;">sign_time</font>`<font style="color:#000000;"> 时，恒有 </font>![image](../../../images/c6413d23fe2477dda374e780edc9803b.svg)<font style="color:#000000;">。</font>

<font style="color:#000000;">即：若在秒数为1时同时调用 </font>`<font style="color:#000000;">sign_time</font>`<font style="color:#000000;">，则一定能得到正确的 </font>`<font style="color:#000000;">secret</font>`<font style="color:#000000;">，在此之后调用 </font>`<font style="color:#000000;">challenge</font>`<font style="color:#000000;"> 提交 </font>![image](../../../images/26d9e6b17dd11dbc59bebbce2d901073.svg)<font style="color:#000000;"> 即可得到正确的</font>`<font style="color:#000000;">flag</font>`<font style="color:#000000;">。</font>

## <font style="color:#5C8D07;">scu_ezxor</font>
<font style="color:#000000;">已知加密信息 S 由 </font>`<font style="color:#000000;background-color:#FFFFFF;">plain</font>`<font style="color:#000000;"> 与 </font>`<font style="color:#000000;background-color:#FFFFFF;">flag</font>`<font style="color:#000000;"> 拼接而来，设将其经过二进制转码后得到 01 数列 </font>![image](../../../images/8d05139d4fff3a901101acdd3747d307.svg)<font style="color:#000000;">，再经过一些操作后最后输出的版本为 </font>![image](../../../images/c1f20528103add93f77e41f5e381ec70.svg)<font style="color:#000000;">。</font>

<font style="color:#000000;">暂时不考虑随机反转的情况，则有：</font>![image](../../../images/db84d481822995c307173eb9245b7b46.svg)<font style="color:#000000;">，其中，</font>![image](../../../images/40acdcad0aabd13ab9e95a8b4ddd0405.svg)<font style="color:#000000;"> 表示异或，</font>![image](../../../images/5b22ebd49ff3a39d8e08cec36761bf48.svg)<font style="color:#000000;"> 表示数列长度。</font>

<font style="color:#000000;">由异或性质，有：</font>![image](../../../images/7fdc1d720003f4e6c9c0398c2b4a7507.svg)<font style="color:#000000;">，无法直接计算的 </font>![image](../../../images/916272fe4237a84075828bd4c1c32038.svg)<font style="color:#000000;"> 可自己枚举 0 或 1——虽然这对解题并不重要。</font>

<font style="color:#000000;">根据上述内容，任意一个 </font>![image](../../../images/2ca8ffc84397b22a46630e29cef74483.svg)<font style="color:#000000;"> 被随机反转只会影响到最多两个 </font>![image](../../../images/ef2cb5ff52a16729d2f3fb8a5a5455e7.svg)<font style="color:#000000;"> 的求出的值。故得出 </font>`<font style="color:#000000;background-color:#FFFFFF;">flag</font>`<font style="color:#000000;"> 部分为：</font>`<font style="color:#000000;background-color:#FFFFFF;">SCUCTF{ucu_i3_s0_o0ol!x0r_1s_8m4z1ngAM</font>`<font style="color:#000000;">,且对应部分的 </font>![image](../../../images/2ca8ffc84397b22a46630e29cef74483.svg)<font style="color:#000000;"> 被改变的数量大约为 5，这意味着其中不正确的字符只有 5 个左右。且通过二进制位而言，每一个位置上可能的正确字符空间不大，可推测每个位置上不正确字符与正确字符的 ASCII 码表示的二进制位差距不超过2。</font>

<font style="color:#000000;">可直接猜测不正确的字符、将其替换成正确的字符然后尝试提交。但更好的方法是：枚举可能的 </font>`<font style="color:#000000;background-color:#FFFFFF;">flag</font>`<font style="color:#000000;">、将其作为随机数种子，然后生成密文与原密文做对比，若二者一致，则 </font>`<font style="color:#000000;background-color:#FFFFFF;">flag</font>`<font style="color:#000000;"> 正确。</font>

<font style="color:#000000;">最终的 </font>`<font style="color:#000000;background-color:#FFFFFF;">flag</font>`<font style="color:#000000;"> 为：</font>`<font style="color:#000000;background-color:#FFFFFF;">SCUCTF{scu_i3_s0_c0ol!x0r_1s_4m4z1ng!}</font>`<font style="color:#000000;">。</font>

