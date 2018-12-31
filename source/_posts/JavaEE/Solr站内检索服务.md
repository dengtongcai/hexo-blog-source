---
layout: '[layout]'
title: Solr站内全文检索服务
date: 2015-06-08 18:36:22
categories: 教程
tags: [框架,站内检索]
---
## Solr服务
Solr 是Apache下的一个顶级开源项目，采用Java开发，它是基于Lucene的全文搜索服务。Solr可以独立运行在Jetty、Tomcat等这些web容器中。
Solr提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展，并对索引、搜索性能进行了优化。
solr和lucene的区别在于：
1. Lucene是一个开放源代码的全文检索引擎工具包，它不是一个完整的全文检索应用。Lucene仅提供了完整的查询引擎和索引引擎，目的是为软件开发人员提供一个简单易用的工具包，以方便的在目标系统中实现全文检索的功能，或者以Lucene为基础构建全文检索应用。
2. Solr的目标是打造一款企业级的搜索引擎系统，它是基于Lucene一个搜索引擎服务，可以独立运行，通过Solr可以非常快速的构建企业的搜索引擎，通过Solr也可以高效的完成站内搜索功能。

## Solr服务的安装
### 独立运行
1. 解压目录
2. 如果测试直接运行`solr/example/start.jar`,这是一个集成在jetty容器下的jar包，可以独立运行。
```
java -jar start.jar
```

### tomcat容器中运行 
我们实际开发和生产环境，都是运行在tomcat容器中，实现步骤为：
1. 解压目录后将`solr/example/webapps/solr.war` 复制到 `apache tomcat/webapps`目录
2. 启动tomcat会报错404：
```
java.lang.NoClassDefFoundError:Failed to initialize Apache Solr:
Could not find necessary SLF4j loogging jar...
```
	缺少运行jar包。我们必须把`solr/example/lib/ext/`目录下的依赖jar包复制到tomcat
3. 再启动后，访问报错500：
```
SolrCore 'collection' is not available due to init failure:
could not loadd load conf for core collection1:
Error loading solr config from solr/collection1\solrconfig.xml...
```
	找不到collection1索引库。我们必须指定solrhome。
	指定solrhome有两种方法：
	- 一是通过tomcat/bin/catlina.bat：`set "JAVA_OPTS=-Dsolr.solr.home=d:/solr"`
	- 二是在tomcat/webapps/solr/web.xml中配置：
```
<env-entry>
	<env-entry-name>solr/home</env-entry-name>
	<env-entry-value>solrhome位置</env-entry-value>
	<env-entry-type>java.lang.String</env-entry-type>
</env-entry>
```
访问：http://localhost：8080/solr

## Solrhome 目录结构
### solrhome
是用来存放索引库的位置。目录结构主要为：
1. bin
2. collection1（索引库）
3. solr.xml(配置solr集群)


### Solrcore
（索引库）目录结构
1. conf（solr配置信息）
schema.xml：用来配置索引的字段以及字段类型，配置方式参照collection1中的配置，主要包括：
	- field（name，type，indexed是否创建索引,stored是否保存,multiValued是豆豆能存储多个值）
	- uniqueKey节点(主键)
	- copyField，将其他字段值copy到目标字段(source，dest)
	- fieldType，字段类型(name：类型名称，class：代表solr的类型，analyzer：指定的分词器，type="index/query"，tokenizer：分词器，filter：指定的过滤器)
	- dynamicField节点：配置动态字段
2. data（存放索引库数据）
3. core.properties（配置索引库访问名称）

### Solrconfig.xml
作用：用来配置索引库的位置以及可以配置solr的添加、删除、更新索引的方法。
1. luceneMatchVersion：指定底层Lucene的版本号信息
2. lib：配置solr服务运行所需要的jar
3. dataDir：指定索引库的位置
4. requestHandler句柄：配置solr服务请求的方法

## 自定义索引库
1. 自定义访问名称
2. 在schem.xml中定义字段，定义切词工具
3. 在solrconfig.xml定义查询句柄

## 从数据库导入数据到索引库
1. 导入jar包`dataimporthandler.jar`
2. 引入jar包
	solrconfig.xml:`<lib dir="../lib/" regex="solr-dataimporthandler-\d.*\.jar" />`
3. 配置导入句柄solrconfig.xml（配置requestHandler）：
```
<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
     <lst name="defaults">
       <str name="config">db-data-config.xml</str>
     </lst>
  </requestHandler>
```
4. ./conf/目录下添加数据库连接信息配置文件：`db-data-config.xml`
5. 将mysql驱动包导入到solr：`mysql-connector-java-5.1.8-bin.jar`
6. 在控制台执行导入

## 索引库的维护（solrj）,日常操作
### 添加索引
```
//1，添加索引
@Test
public void createIndex() throws SolrServerException, IOException{
	//连接solr
	String baseURL = "http://localhost:8080/solr/article";
	HttpSolrServer httpSolrServer = new HttpSolrServer(baseURL);
	//创建SolrInputDocument对象
	SolrInputDocument solrInputDocument = new SolrInputDocument();
	solrInputDocument.addField("id", "1");
	solrInputDocument.addField("title", "索引");
	solrInputDocument.addField("content", "这里写的是内容");
	//添加索引
	httpSolrServer.add(solrInputDocument);
	httpSolrServer.commit();
}
```
### 更新索引
执行添加，id一样表示更新
### 检索索引
```
//2，简单查询
@Test
public void queryIndex() throws SolrServerException, IOException{
	//连接solr
	String baseURL = "http://localhost:8080/solr/article";
	HttpSolrServer httpSolrServer = new HttpSolrServer(baseURL);
	//通过SolrQuery封装查询对象
	SolrQuery params = new SolrQuery();
	params.setQuery("海军");
	params.set("df", "common_query");
	//执行查询
	QueryResponse queryResponse = httpSolrServer.query(params);
	SolrDocumentList results = queryResponse.getResults();
	
	System.out.println("----------");
	System.err.println(results.getNumFound());
	for (SolrDocument doc : results) {
		System.err.println(doc.get("id"));
		System.err.println(doc.get("title"));
		System.err.println(doc.get("content"));
	}
}
```

### 关键字高亮
```
//3，高亮
@Test
public void queryIndexhl() throws SolrServerException, IOException{
	//连接solr
	String baseURL = "http://localhost:8080/solr/article";
	HttpSolrServer httpSolrServer = new HttpSolrServer(baseURL);
	//通过SolrQuery封装查询对象
	SolrQuery params = new SolrQuery();
	params.setQuery("title:小米");
	// 开启高亮器
	params.setHighlight(true); // 开启高亮
	params.addHighlightField("content");// 添加高亮的字段
	params.setHighlightSimplePre("<em>");// 设置开始标签
	params.setHighlightSimplePost("</em>");// 设置结束标签
	//执行查询
	QueryResponse queryResponse = httpSolrServer.query(params);
	SolrDocumentList results = queryResponse.getResults();// 普通的结果集
	
	//获取高亮结果
	Map<String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();
	System.out.println("----------");
	for (SolrDocument doc : results) {
		System.err.println(doc.get("id"));
		System.err.println(doc.get("title"));
		System.err.println(doc.get("content"));
		System.out.println(highlighting.get(doc.get("id")).get("content").get(0));
	}
	
}
```
### 删除索引
```
@Test
public void testDeleteById() throws SolrServerException, IOException{
	// 1、连接solr服务
	String baseURL = "http://localhost:8018/solr/article";
	HttpSolrServer httpSolrServer = new HttpSolrServer(baseURL);
	
	// 2、根据id删除
//		httpSolrServer.deleteById("3");
	
	//3，先查后删
	httpSolrServer.deleteByQuery("title:手机");
	httpSolrServer.commit();
}
```
