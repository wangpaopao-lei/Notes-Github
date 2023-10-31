# 第三章 Neo4j 测试2



1)

```cypher
MATCH (n)-[*..5]->(m)
WHERE Id(n)=123
RETURN DISTINCT n;
```

2）

查询有 name 属性值为"wang"的 Student 节点到 name 属性值为"NoSQL"的节点的类别为 Study 的关系，如果没有匹配的节点或关系就创建；将关系的 year 属性值修改为 2023，返回关系。

3）

 匹配 name 为吴京的 Person 节点和 name 为 The Wandering Earth II 的 Movie 节点，创建吴京到流浪地球2 的关系，关系的类型为 ACTED_IN，关系有 roles 属性，值为一个数组类型，数组中包含一个'Liu Qi'元素。

