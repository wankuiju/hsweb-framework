# 通用Controller
提供增删改查的restful接口

# 动态查询

目前支持动态查询条件类 `QueryParamEntity`:
前端传参数:
1. 普通条件
```html
terms[0].column=name&terms[0].termType=like&terms[0].value=张三
```
等同于sql
```sql
where name like ?
where name like '张三'
```

2. 复杂条件
```html
terms[0].column=name&terms[0].termType=eq&terms[0].value=张三
&terms[1].column=name&terms[1].termType=eq&terms[1].type=or&terms[1].value=李四
```
等同于sql
```sql
where name =? or name = ?
where name = '张三' or name = '李四'
```

3. 嵌套条件
```html
terms[0].column=name&terms[0].termType=like&terms[0].value=张%
&terms[1].type=and
&terms[1].terms[0].column=age&terms[1].terms[0].termType=gt&terms[1].terms[0].value=10
&terms[1].terms[1].column=age&terms[1].terms[1].termType=lt&terms[1].terms[1].value=18

```
等同于sql
```sql
where name like ? and (age>? and age <?)
where name like '张%' and (age>10 and age <18)

```

4. 排序
```html
sorts[0].name=age&sorts[0].order=desc
```
等同于sql
```sql
order by age desc 
```

5. 分页
```html
pageIndex=0&pageSize=20
```

不分页查询
```html
paging=false
```

6. 指定要查询的列
```html
includes=id,name,age
```
等同于sql
```sql
select id,name,age from ......
```
不查询的列参数为excludes,如:`excludes=comment,phone`

注意: 以上参数都进行了验证,不会有sql注入问题。
