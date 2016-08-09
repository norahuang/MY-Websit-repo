---
layout: post
title:  "maven environment setup on windows"
date:   2016-08-09 12:03:30 -0700
categories: Technology
---


<p> 1. setup java environment varible in usr environment varible</p>
<p>    JAVA_HOME to java install diretory</p>
<p>	append ;%JAVA_HOME% to path</p>
<p>2. setup maven environment varible in usr environment varible</p>
<p>   M2_HOME=C:\Program Files\Apache Software Foundation\apache-maven-3.3.3</p>
<p>   M2=%M2_HOME%\bin</p>
<p>   MAVEN_OPTS=-Xms256m -Xmx512m</p>
<p>   append ;%M2% to path</p>



