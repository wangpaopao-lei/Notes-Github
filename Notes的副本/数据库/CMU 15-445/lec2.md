advanced sql



sequel  结构化查询语言

关系模型、关系代数

DML（数据操作语言）、DDL、DCL的集合

### Agggregations

输入一个 bag 的记录，输入一个值

1. avg
2. min
3. max
4. sum
5. count

```sqlite
SELECT COUNT(login) AS cnt
	FROM student WHERE login LIKE '@cs'
```

支持关键字 distinct，输出不相同的记录个数

### Group by

```sqlite
SELECT AVG(s.gpa), e.cid
FROM enrolled AS e, student AS s
WHERE e.sid = s.sid
GROUP BY e.cid;
```

having 关键字对输出结果进行限制



