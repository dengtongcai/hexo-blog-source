layout: '[layout]'
title: Solr站内检索服务搭建常见问题
date: 2015-06-10 12:16:32
categories: Lucene
tags: [BUG,站内检索]
---
在自定义索引库时，这时需要添加collection，添加的collection配置需要与生产环境保持一致，于是复制默认的collection1 的配置信息作为新的collection复制完成，新的collection events也添加完成；但是加载时总是报错不能正确加载solrconfig.xml信息，也知道schema.xml等数据肯定是要修改的，schema.xml配置信息修改完成后还是有这样的问题，在往solr写入数据时又再次报错undefined filed message，但是message字段确实已经配置好了；检查之后再次重启solr，查看刚才events 直接显示 “There exists no core with name "events",
这时去查看日志，显示信息使用的主键类型是Long，启动后报错：
```
org.apache.solr.common.SolrExceptiom:Errorinitializing Query Elevation Component;
caused by:java.lang.NumberFormateException:for input string:”MA47LL/A”.
```
大致的意思是指定的类型应该是string，而实际不是，导致包数字转换异常。自己折腾半天没有什么效果，果断google了一下，得到了解决方案：
如果你开启了 QueryElevationComponent 功能，但是schema 的uniqueKey类型又不是 string，则报如下错误:
```
java.lang.NumberFormatException: For input string: "MA147LL/A"
```
这个不就是我的日志里面的那个错误信息么，于是编辑`example/solr/events/conf/solrconfig.xml`配置文件搜索`QueryElevationComponent`关键字，可以看到如下，果然有这个信息。类似于关键字搜索后，一些项的配置置顶显示 比如百度搜索某个关键字时，搜索框下面的推广，广告相关信息总是被置顶显示，要配置启用这项组件，需要配置elevate.xml，如果不需要，注释即可：
```
<searchComponent name="elevator" class="solr.QueryElevationComponent" >
    <str name="queryFieldType">string</str>
    <str name="config-file">elevate.xml</str>
  </searchComponent>
  <!-- A request handler for demonstrating the elevator component -->
  <requestHandler name="/elevate" class="solr.SearchHandler" startup="lazy">
    <lst name="defaults">
      <str name="echoParams">explicit</str>
      <str name="df">text</str>
    </lst>
    <arr name="last-components">
      <str>elevator</str>
    </arr>
 </requestHandler>

```

## solr启动时报错
solr启动时报错：`org.apache.solr.common.SolrException : undefined field text`
虽然这个错误基本使用没有发现什么异常结果，新惯性看一下logging，发现有个没见过的错误，google后发现：
在schema.xml 定义中不存在text field ，在启动solr 时会出现下面的异常:`org.apache.solr.common.SolrException: undefined field text`
参考：http://blog.csdn.net/jaylong35/article/details/9031075
解决方法为找到这段配置：
```
<listener event="newSearcher" class="solr.QuerySenderListener">
    <arr name="queries"></arr>
    </listener>
<listener event="firstSearcher" class="solr.QuerySenderListener">
      <arr name="queries">
        <lst>
          <str name="q">static firstSearcher warming in solrconfig.xml</str>
        </lst>
      </arr>
</listener>
```
把`<str name="q">static firstSearcher warming in solrconfig.xml</str>`
改成`<str name="q">*:*</str>`

```
<listener event="newSearcher" class="solr.QuerySenderListener">
    <arr name="queries"></arr>
    </listener>
<listener event="firstSearcher" class="solr.QuerySenderListener">
      <arr name="queries">
        <lst>
          <str name="q">static firstSearcher warming in solrconfig.xml</str>
        </lst>
      </arr>
</listener>
```