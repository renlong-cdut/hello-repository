

文本换行：  
方法：文本末尾至少空两格，再按enter键才行。  


标题demo：  #号与内容之间必须有一个空格
# 一级标题
## 二级标题
### 三级标题
#### 四级标题     


列表demo：序号与内容之间必须有一个空格
1. 天天  //1.后必须空格才是列表功能，否则就是文本  
2. gg  
3. dd
	- ee
	- hh
		- tt  
		- gg  
		
文本内容分割线：  
至少输入3个下滑线或者破折线或者*号   
     
----------     



输入代码：  
``可以表示输入一行代码，可是第一行和第二行代码区域块是分开的，不在一个代码区域块里。  

`int i=2;float r=8.2; int k=9;`     
`int j=4;`   
 
tab键可以表示输入一行代码，每行代码在一块区域块里。       
 
    include ""
	haha
	tt
	dfhdf
   


hhh  

	int i =2;  
	int b=6;  


```  ```：表示输入一段代码，但是必须通过MarkdownPad2的设置才能实现，参考[知乎:Markdownpad2如果实现代码换行](https://www.zhihu.com/question/29383702),具体操作步骤如下：
Tool -> Options -> markdown -> Markdown Processor 设置为 CommonMark

这样设置后使用```的代码块在Markdown中查看可以实现换行。否则```之间的代码显示成一行，没法实现换行。另外将Markdown Processor 设置为 CommonMark,MarkdownPad2可以支持下划线“_”，默认的Markdown Processor不支持下划线“_”。
```
	int  i=1;  
	int b=3;
```  


文字斜体：  
*gggg*   
  
文字粗体：  
**hhhh**  
**ff **    

文字加粗、斜体：  
***hhh***    
ggg***hhh***  

文字高亮：  
`hhh`  
`ggg`    

链接：   
格式：[链接文字](链接地址)  
例子：    
[Markdown](http://blog.csdn.net/zhaokaiqiang1992)    

图片：  
格式：![Alt text](/path/to/img.jpg)  
详细叙述如下：  
一个惊叹号 !  
接着一个方括号，里面放上图片的替代文字  
接着一个普通括号，里面放上图片的网址      
  
引用：  
>一级引用  
>>二级引用  




