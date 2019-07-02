# Neo4j

1.增

* 1.1 创建节点
		create (n:"人物"{name:"特朗普","职位":"总统"}) return n

* 1.2 创建节点间关系
    match(n1:"人物"{name:"特朗普"}),(n2{name:"拿破仑"})
    create (n1)-[r:"弱相关]-(n2) return r


* 1.3 创建节点关系和属性
    match(n1:"人物"{name:"特朗普"}),(n2{name:"伊万卡"})
   *create (n1)-[r:"弱相关{name:"父女"}]-(n2) return r

* 1.4 创建完整路径
    create (n1:"人物"{name:"特朗普"})-[r:"弱相关"{name:"父女"]-(n2:"人物"{name:"伊万卡"}) 
    return n1,n2,r


2.删
* 2.1 删除所有节点与关系——delete
    根据属性删除单个节点：  match(n:"人物"{name:"特朗普"}) delete n
    根据属性删除单个关系：  match(n1:"人物{name:"特朗普"})-[r:"强相关"]-(n2) 
                         where n2.name="拿破仑 delete r
    根据id删除节点：       match(n) where id(n)=2134 delete n
    根据id删除节点关系：    match(n)-[r]-() where id(r)=1234 delete r
    删除某一类关系：        match(n)-[r:"强相关"]-() delete r 

* 2.2 删除标签与属性——remove
    删除单节点的某个属性：  match(n{name:"特朗普"}) remove n."职位" return n
    删除节点的标签：       match(n{name:"特朗普"}) remove n:"人物" return n
    删除多重的标签：       match(n{name:"特朗普"}) remove n:"人物":"Person" return n    

* 2.3 重设为NULL——set
    删除属性：            match(n{name:"特朗普"}) set n."职位"=NULL return n
    
3.改
    改额外标签：          match (n) where id(n)=127 set n:"Person" return n
    改额外属性：          match (n) where id(n)=128 set n.age = "12" return n
    设置多个属性：        match (n) where id(n)=128 set n.age = "12"，n."职位" = "学生" return n
    
4.查
   * 4.1 查节点：              match(n:"人物"{name:"特朗普")  return n
                         或者 match(n:"人物") where n.name = "特朗普" return n
    限制显示条数：         match(n:"人物") return n limit 20
    查关系：              match(n1{name:"特朗普"})-[r]-(n2) return r
    查关系限制标签：        match(n1{name:"特朗普"})-[r:"强相关"]-(n2) return r
    查询关系属性和类型：    match (p1:"人物"{ name:"特朗普})-[r]->(p2) return r,type(r)
    
    
*4.2 集合函数查询

（1）通过id函数，返回节点或关系的ID
MATCH (:Person { name: 'Oliver Stone' })-[r]->(movie)
RETURN id(r);

（2）通过type函数，查询关系的类型
MATCH (:Person { name: 'Oliver Stone' })-[r]->(movie)
RETURN type(r);

（3）通过lables函数，查询节点的标签
MATCH (:Person { name: 'Oliver Stone' })-[r]->(movie)
RETURN lables(movie);

（4）通过keys函数，查看节点或关系的属性键
MATCH (a)
WHERE a.name = 'Alice'
RETURN keys(a)

（5）通过properties()函数，查看节点或关系的属性
CREATE (p:Person { name: 'Stefan', city: 'Berlin' })
RETURN properties(p)

（6）nodes(path)：返回path中节点
match p=(a)-->(b)-->(c) where a.name='Alice' and c.name='Eskil' return nodes(p)

（7）relationships(path)：返回path中的关系
match p=(a)-->(b)-->(c) where a.name='Alice' and c.name='Eskil' return relationships(p)

* 4.3 路径查询：
常规路径查询：
match (:"人物"{name:"特朗普"})->(p) return p

可变长路径查询： 
match (:"人物"{name:"特朗普"})->[r*1..4]->(p) return r,p #距离为1到4
 
最短路径查询：
match path = shortestPath( (p1:"人物"{name:"特朗普"})->[r*1..4]->(p2) ）
where p2.name="拿破仑"
return path,length(path)




python调用：
