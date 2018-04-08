# tech-view  

google chrome extension and a very light tech info site build with vue+muse-ui  
基于vue+muse-ui+readhub的展api做的一款非常简单的谷歌扩工具,用于查看科技、媒体、区块链相关新闻.  

####下载：
[扩展程序谷歌商店](https://chrome.google.com/webstore/detail/tech-view/mdjdpkdjblhjgpcglocodphajghbjdfn)  

####网址:  
[Tech-view](http://111.231.70.202/)

####截图
![shot1]("./shots/banner1.png")  

![shot2]("./shots/shot1.png")  

####开始:
<code>npm install</code>  
<code>npm run dev</code> 

####生成静态网页:  
<code>npm run build</code>  

####Tips:  
在项目开发过程中遇到3点值得注意的地方,  
1.如果不考虑兼容性,flex布局真是个不错的选择,  
2.-webkit-overflow-scrolling : touch;当滚动内容不是在body上的时候，增加快速滚动和回弹效果,  
3.在适应footer高度的时候,为了防止调用过多,用到了js函数去抖    
#####待改善:    
1.引入muse-ui组件的时候可以按需引入,免得项目整体太重了,    
2.在适应手机端的时候,有些机型字体看起来太小,应该使用REM布局,  
3.为了不过度使用api,设定的是每隔30s获取一次数据,但是没有考虑到实际使用的情况(在某些条件下,每次点开需要获取一次数据)   
