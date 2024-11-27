队伍：<font style="background-color:rgba(255, 255, 255, 0);">Aura&Jumping</font>

## <font style="color:rgb(107, 0, 64);">P</font><font style="color:rgb(0, 54, 111);">PC</font>
### BaseCTF 崩啦 - 导言
如题，下载好附件后在 README.txt 中发现 flag。

### BaseCTF 崩啦 II - 过期了?
要求找到 `Damien Schroeder` 队伍的所有逾期提交题目，并按照时间顺序用英文逗号连接，取 md5 值。

看了一下，所有题目的截止提交时间都为 2024-08-31T15:00:00+08:00，也就是找该队伍 2024.8.31 15:00:00 后的所有提交记录，找到以后通过题目名称去对应题目的 Id，排序以后是这样。

:::info
[2024/8/31 16:09:34 +08:00 INF] 8bcad0d3-6379-411f-5edb-650c20f30dd5



[2024/8/31 17:38:58 +08:00 INF] 563c8303-5eb1-affe-1369-e8889b9323eb



[2024/8/31 18:07:48 +08:00 INF]  ef0a3095-ece1-c427-ab20-99a79fe10bc8



[2024/8/31 18:30:22 +08:00 INF] 2ac9055a-1a79-857f-ed12-e33e5acdc9a3



[2024/8/31 20:28:55 +08:00 INF]  063c3506-d3e4-668b-88f7-2da39092dfe0



[2024/8/31 23:33:54 +08:00 INF] b18048d0-bd8e-0fd0-22b3-61780bae1608

:::

按照要求拼接后长这样。

:::info
8bcad0d3-6379-411f-5edb-650c20f30dd5,563c8303-5eb1-affe-1369-e8889b9323eb,ef0a3095-ece1-c427-ab20-99a79fe10bc8,2ac9055a-1a79-857f-ed12-e33e5acdc9a3,063c3506-d3e4-668b-88f7-2da39092dfe0,b18048d0-bd8e-0fd0-22b3-61780bae1608

:::

md5 后套上 BaseCTF 就是 flag。

### BaseCTF 崩啦 III - 帮我查查队伍的解题
比起上一题，手动去做显得更吃力，所以需要借助一下脚本。

大致意思就是找到  Rick Hyatt  队伍所有未提交，提交错误，或者逾期提交的所有题目，按照 challenges.json 中的题目顺序拼接，取 md5 值。

正难则反，我们把该队伍所有提交正确的题目找出来，然后把这些题目作为黑名单，用脚本跑一下，自然就能得到想要的那些题目。

可以手动把该队伍的提交记录都取出来，先删掉逾期提交的题目，再去看剩余的题目是否有提交错误的。

怎么看提交是否正确，这里需要先去看 teams.json，找到该队伍的 Id，然后在 flags.json 把该队伍的 flag 提出来，依次对比即可。

最后只有 3 个题目提交正确，把他们写入一个 black.txt，作为黑名单。

```plain
[[Week2] Aura 酱的旅行日记 VI <图寻擂台>]
[[Week3] 你为什么不让我溢出]
[[Week2] try_to_factor]
```

接下来用脚本跑，把所有未在黑名单中的题目的 Id 拼出来即可。

```python
import json
from hashlib import md5
with open('black.txt', 'r', encoding='utf-8') as file:
    blacklist = file.read()
with open('challenges.json', 'r') as file:
    data = json.load(file)
ids_to_output = []
cnt = 0
for item in data:
    if item['Name'] not in blacklist:
        ids_to_output.append(item['Id'])
        cnt += 1
output_ids = ','.join(ids_to_output)
print(output_ids)
print(cnt)
print(md5(output_ids.encode('utf-8')).hexdigest())
```

最终 md5 值：4f87919032dfc58a9a23044aab9acd99。

包上 BaseCTF 就是 flag。

### BaseCTF 崩啦 IV - 排排坐吃果果
统计排名，细节很多。

+ 超时提交的不计分
+ 0 分的队伍也要计算到排名中，但有可能这类队伍就没提交记录，需要从 teams.json 里面初始化
+ 有的队伍会重复提交正确的 flag，分数不能重复累加
+ 有个重名的队伍，不过出题人很良心，把这个重名队伍的得分合并也视为正确答案

为了方便书写计算代码，这里把 submissions.log 通过脚本转成了 submit.json，同时忽略了超时提交的记录。

```python
import re
from datetime import datetime
import json
comparison_time_str = "2024-08-31T15:00:00+08:00"
comparison_time = datetime.fromisoformat(comparison_time_str)
with open('submissions.log', 'r') as f:
    log_entries = f.read()
log_entries = log_entries.split('\n')
logs = []
def extract_info(log_line):
    patterns = {
        'time': r'\[(.*?)\]',
        'team': r'队伍 \[(.*?)\]',
        'challenge': r'提交题目 \[(.*?)\] 的答案',
        'flag': r'答案 \[(.*?)\]'
    }
    extracted_info = {key: re.search(pattern, log_line).group(1) if re.search(pattern, log_line) else None
                      for key, pattern in patterns.items()}
    return extracted_info
for log in log_entries:
    log_json = extract_info(log)
    if log_json['time']:
        log_json['time'] = time = datetime.strptime(log_json['time'][:-4], '%Y/%m/%d %H:%M:%S %z')
        if log_json['time'] <= comparison_time:
            log_json.pop('time', None)
            logs.append(log_json)
def save_to_json(data, filename):
    with open(filename, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=True, indent=3)
save_to_json(logs, 'submit.json')
```

生成了 submit.json 以后，接下来就是统计排名，需要把之前的细节都考虑到。

对于重复提交正确 flag 的队伍，这里使用 vis 字典进行打标记，未打标记的正常加分，打过标记的不再计分。

```python
import json
import hashlib
from tqdm import tqdm
with open('challenges.json', 'r') as f:
    json_string = f.read()
challenges = json.loads(json_string)
with open('flags.json', 'r') as f:
    json_string = f.read()
flags = json.loads(json_string)
with open('teams.json', 'r') as f:
    json_string = f.read()
teams = json.loads(json_string)
with open('submit.json', 'r') as f:
    json_string = f.read()
submits = json.loads(json_string)
scores = {}
vis = {}
for team in teams:
    scores[team['Name']] = 0
    vis[team['Name']] = {}
    for problem in challenges:
        vis[team['Name']][problem['Id']] = False
for submit in tqdm(submits, desc="Processing submits"):
    sub_team = submit['team']
    sub_flag = submit['flag']
    sub_challenge = submit['challenge']
    challenge = next((item for item in challenges if item['Name'] == sub_challenge), None)
    points = challenge['Points']
    challenge_id = challenge['Id']
    team = next((item for item in teams if item['Name'] == sub_team), None)
    team_id = team['Id']
    if vis[sub_team][challenge_id]:
        continue
    flag = next((item for item in flags if (item['ChallengeId'] == challenge_id and item['TeamId'] == team_id)), None)
    if flag['Flag'] == sub_flag:
        vis[sub_team][challenge_id] = True
        scores[sub_team] += points
def sort_and_generate_flag(data):
    sorted_data = sorted(data.items(), key=lambda x: (-x[1], x[0]))
    concatenated_string = ';'.join(f"{team},{points}" for team, points in sorted_data)
    print(concatenated_string)
    md5_result = hashlib.md5(concatenated_string.encode('utf-8')).hexdigest()
    flag = f"BaseCTF{{{md5_result}}}"
    return flag
print(sort_and_generate_flag(scores))
```



### BaseCTF 崩啦 V - 正义执行!
统计作弊队伍，只要提交其他队伍 Flag 都视为作弊。

需要将之前转 json 的脚本修改一下，把时间过滤删去即可。

```python
import re
import json
with open('submissions.log', 'r', encoding='utf-8') as f:
    log_entries = f.read()
log_entries = log_entries.split('\n')
logs = []
def extract_info(log_line):
    patterns = {
        'time': r'\[(.*?)\]',
        'team': r'队伍 \[(.*?)\]',
        'challenge': r'提交题目 \[(.*?)\] 的答案',
        'flag': r'答案 \[(.*?)\]'
    }
    extracted_info = {key: re.search(pattern, log_line).group(1) if re.search(pattern, log_line) else None
                      for key, pattern in patterns.items()}
    return extracted_info
for log in log_entries:
    log_json = extract_info(log)
    logs.append(log_json)
def save_to_json(data, filename):
    with open(filename, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=True, indent=3)
save_to_json(logs, 'submit.json')

```

统计作弊队伍也有一些小细节。

+ 最后要的是未作弊的队伍
+ 有可能有的队伍交了多个其它队伍的 flag
+ 只要交了其它队伍的 flag，不管是哪个题，双方都视为作弊

```python
import json
import hashlib
from pwnlib.dynelf import sizeof
from tqdm import tqdm
with open('challenges.json', 'r') as f:
    json_string = f.read()
challenges = json.loads(json_string)
with open('flags.json', 'r') as f:
    json_string = f.read()
flags = json.loads(json_string)
with open('teams.json', 'r') as f:
    json_string = f.read()
teams = json.loads(json_string)
with open('submit.json', 'r') as f:
    json_string = f.read()
submits = json.loads(json_string)
cheaters = []
all_teams = []
for team in teams:
    all_teams.append(team['Id'])
for submit in tqdm(submits, desc="Processing submits"):
    sub_team = submit['team']
    sub_flag = submit['flag']
    sub_challenge = submit['challenge']
    # challenge = next((item for item in challenges if item['Name'] == sub_challenge), None)
    # challenge_id = challenge['Id']
    team = next((item for item in teams if item['Name'] == sub_team), None)
    team_id = team['Id']
    flag = next((item for item in flags if (item['Flag'] == sub_flag)), None)
    if flag is None:
        continue
    if flag['TeamId']!=team_id:
        if team_id not in cheaters:
            cheaters.append(team_id)
        if flag['TeamId'] not in cheaters:
            cheaters.append(flag['TeamId'])

print(cheaters)
def sort_and_generate_flag():
    temp = []
    for team in all_teams:
        if team in cheaters:
            continue
        temp.append(team)
    ans = []
    for id in temp:
        team = next((item for item in teams if item['Id'] == id), None)
        ans.append(team['Name'])
    ans.sort()
    concatenated_string = ','.join(team for team in ans)
    print(concatenated_string)
    md5_result = hashlib.md5(concatenated_string.encode('utf-8')).hexdigest()
    flag = f"BaseCTF{{{md5_result}}}"
    return flag
print(sort_and_generate_flag())
```

:::color4
Addie Will,Amber Powlowski,Annabelle Treutel,Camilla Halvorson,Candice Rempel,Einar Langosh,Gerson Reynolds,Guy Stiedemann,Jackson Beatty,Janiya Boehm,Jasen Weimann,Jewel Mohr,Kathlyn Wintheiser,Lexie O'Conner,Marquis Metz,Mikayla Lubowitz,Molly Rodriguez,Nellie Purdy,Queenie Franecki,Rick Auer,Rosendo Jenkins,Tess Koelpin,Wilhelmine Smith

:::

👆所有未作弊的队伍（按队名字典序排序）



