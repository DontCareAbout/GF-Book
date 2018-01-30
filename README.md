GF（Goddamn Fuckwork）是 [Don'tCare] 在 WTF/min 指數持續飆高的開發過程中，
設法讓自己好過一點的雜亂 class 集合。
也許有一天會演化到 framework 的規模，
看看河內之塔完結篇之前有沒有機會。

GF 主要奮戰的對象是 [GWT] 與 [GXT]，
連帶也設法解決一些 Java 問題。

GF 目前預設的環境如下：

* Java 1.7
* GWT 2.7
* GXT 3.1
* Spring 4.1.9
* Guava 19


使用方式
========

Maven
-----

```XML
	<dependency>
		<groupId>us.dontcareabout</groupId>
		<artifactId>gf</artifactId>
		<version>0.0.2</version>
	</dependency>
```

> ###### 注意 ######
> 目前 Maven Repo 並沒有 GF，
> 請自己 clone [GitHub repo][GF] 然後 `mvn install`...... Orz


GWT module
----------

在 gwt.xml 當中加入

```XML
<inherits name="us.dontcareabout.gwt.GWT" />
```


GXT module
----------

在 gwt.xml 當中加入

```XML
<inherits name="us.dontcareabout.gxt.GXT" />
```

這會載入下列 module：

* us.dontcareabout.gwt.GWT
* com.sencha.gxt.ui.GXT
* com.sencha.gxt.chart.Chart


Feedback？
==========

無論是想貢獻程式碼、許願新功能、或是詰譙 bug，都十分歡迎，
請到 [GF 的 GitHub repo][GF] 上頭發 pull request 或是 issue。

如果是針對文件本身的改進 / 抱怨，
也可以直接到[這個文件的 GitHub repo][GF book] 發 pull request 或是 issue。


[Don'tCare]: https://github.com/DontCareAbout
[GWT]: http://www.gwtproject.org/
[GXT]: https://www.sencha.com/products/gxt/
[GF]: https://github.com/DontCareAbout/gf
[GF book]: https://github.com/DontCareAbout/GF-Book