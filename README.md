# Neo4j

1.增


* 1.1 创建节点<br>
		create (n:"人物"{name:"特朗普","职位":"总统"}) return n

* 1.2 创建节点间关系<br>
    match(n1:"人物"{name:"特朗普"}),(n2{name:"拿破仑"})<br>
    create (n1)-[r:"弱相关]-(n2) return r<br>


* 1.3 创建节点关系和属性<br>
    match(n1:"人物"{name:"特朗普"}),(n2{name:"伊万卡"})<br>
   *create (n1)-[r:"弱相关{name:"父女"}]-(n2) return r<br>

* 1.4 创建完整路径<br>
    create (n1:"人物"{name:"特朗普"})-[r:"弱相关"{name:"父女"]-(n2:"人物"{name:"伊万卡"}) <br>
    return n1,n2,r<br>



2.删

* 2.1 删除所有节点与关系——delete<br>
    根据属性删除单个节点：<br>
    match(n:"人物"{name:"特朗普"}) delete n<br>
    根据属性删除单个关系：<br>
    match(n1:"人物{name:"特朗普"})-[r:"强相关"]-(n2) 
                         where n2.name="拿破仑 delete r<br>
    根据id删除节点：<br>
    match(n) where id(n)=2134 delete n<br>
    根据id删除节点关系：  <br><br>
    match(n)-[r]-() where id(r)=1234 delete r<br>
    删除某一类关系： <br>
    match(n)-[r:"强相关"]-() delete r <br>

* 2.2 删除标签与属性——remove<br>
    删除单节点的某个属性： <br>
    match(n{name:"特朗普"}) remove n."职位" return n<br>
    删除节点的标签： <br>
    match(n{name:"特朗普"}) remove n:"人物" return n<br>
    删除多重的标签： <br>
    match(n{name:"特朗普"}) remove n:"人物":"Person" return n   <br> 
    
* 2.3 重设为NULL——set<br>
    删除属性：            match(n{name:"特朗普"}) set n."职位"=NULL return n<br>
    
3.改<br>
* 3.1 改节点标签和属性<br>
    加额外标签：<br>
    match (n) where id(n)=127 set n:"Person" return n<br>
    加额外属性： <br>
    match (n) where id(n)=128 set n.age = "12" return n<br>
    加多个属性：<br>
    match (n) where id(n)=128 set n.age = "12"，n."职位" = "学生" return n<br>

4.查<br>
* 4.1 查节点和关系<br>
    查节点：<br>
    match(n:"人物"{name:"特朗普")  return n<br>
                         或者 match(n:"人物") where n.name = "特朗普" return n<br>
    限制显示条数：<br>
    match(n:"人物") return n limit 20<br>
    查关系：<br> 
    match(n1{name:"特朗普"})-[r]-(n2) return r<br>
    查关系限制标签：<br>
    match(n1{name:"特朗普"})-[r:"强相关"]-(n2) return r<br>
    查询关系属性和类型：<br>
    match (p1:"人物"{ name:"特朗普})-[r]->(p2) return r,type(r)<br>

* 4.2 集合函数查询<br>

    通过id函数，返回节点或关系的ID<br>
    MATCH (:Person { name: 'Oliver Stone' })-[r]->(movie)<br>
    RETURN id(r);<br>
  
    通过type函数，查询关系的类型<br>
    MATCH (:Person { name: 'Oliver Stone' })-[r]->(movie)<br>
    RETURN type(r);<br>

    通过lables函数，查询节点的标签<br>
    MATCH (:Person { name: 'Oliver Stone' })-[r]->(movie)<br>
    RETURN lables(movie);<br>

    通过keys函数，查看节点或关系的属性键<br>
    MATCH (a)<br>
    WHERE a.name = 'Alice'<br>
    RETURN keys(a)<br>

    通过properties()函数，查看节点或关系的属性<br>
    CREATE (p:Person { name: 'Stefan', city: 'Berlin' })<br>
    RETURN properties(p)<br>

    nodes(path)：返回path中节点<br>
    match p=(a)-->(b)-->(c) where a.name='Alice' and c.name='Eskil' return nodes(p)<br>

    relationships(path)：返回path中的关系<br>
    match p=(a)-->(b)-->(c) where a.name='Alice' and c.name='Eskil' return relationships(p)<br>

* 4.3 路径查询：<br>
    常规路径查询：<br>
    match (:"人物"{name:"特朗普"})->(p) return p<br>

    可变长路径查询： <br>
    match (:"人物"{name:"特朗普"})->[r*1..4]->(p) return r,p #距离为1到4<br>
  
    最短路径查询：<br>
    match path = shortestPath( (p1:"人物"{name:"特朗普"})->[r*1..4]->(p2) <br>
    where p2.name="拿破仑"<br>
    return path,length(path)<br>


参考：
https://blog.csdn.net/sinat_26917383/article/details/79883503
https://blog.csdn.net/qq_40036754/article/details/88605030
