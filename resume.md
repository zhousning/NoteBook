工作经历
2015年7月——至今 吉林市美联科技有限公司
  基于Kindeditor开发“快论文”（www.kuai65.com）智能编辑器，完全负责前端开发工作
  基于Ionic+AngularJs开发“快论文App”
  基于Ruby on Rails开发白鸽信使微信公众号
  撰写快论文、快论文app、白鸽信使软件著作权
2014年10月——2015年3月  吉林市美联科技有限公司
  基于Ruby on Rails框架开发在"家教呢"(www.jiajiaone.com)线家教O2O平台
  基于Bootstrap开发“家教呢”微信公众号
  撰写家教呢软件著作权
2014年4月——2014年9月  国电江南热电厂
  参与开发电力安全知识培训系统
2013年8月——2014年4月  国网吉林供电公司
  参与开发电力安全规程多媒体网校系统，使用java+spring+Struts+hibernate

开源项目和作品

技能清单
  精通：Ruby on Rails框架、HTML5、CSS3、JavaScript、Bootstrap、Jquery、Kindeditor
  熟悉：JAVA、Spring、Hibernate、Struts、ionic、AngularJS、GitHub、SVN
  了解：Bower/Gulp/SaSS/LeSS/PhoneGap



主要工作内容：

系统设计：

​	用户交互、系统UI

系统研发：

​	1.按后台数据协议，编辑器保留需要标签和属性，清除非需要标签和属性

​	2.基于mathml创建kindeditor公式插件

​	3.按格式规则和数据结构规则，实现对编辑区内容自动排版

​	4.按数据结构规则要求，实现多种来源（网页、word、excel、ppt、wps文档）的文本、图片、表格复制黏贴

​	5.编辑区内内容复制黏贴，保证符合数据结构规则要求

​	6.编写参考文献标号自动生成算法，可以按照用户选定参考文献，自动生成标准格式参考文献标号，如[1]、[1,3,5]、[1-3]

​	7.实现论文分页预览功能

​	8.二次开发kindeditor标题设置功能，使得标题设置能够根据格式规则要求，设置标题样式

​	9.二次开发kineditor图片和表格插件，按需要设置功能

总结：我所做的所有工作就是要保证用户在编辑区内无论进行任何操作，都能够按格式规则和数据结构要求生成需要内容。示例：

<h1 data-type="title-one">一级标题</h1>

<h2 data-type="title-two">二级标题</h2>

<h3 data-type="title-one">三级<sup>[1,3,5]</sup]标题</h3>

<div data-type="thesis-image">

​	<div data-type="thesis-image-tool">tool content</div>

​	<img src="example.jgp"/>

</div>