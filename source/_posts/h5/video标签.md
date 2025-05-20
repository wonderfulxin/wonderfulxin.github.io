---
date: 2016-12-23 11:35
status: public
title: html5 Video标签实现
tags:
  - H5
---

摘要:H5标签实现  在播放视频流的时候会自动全屏，可以实现不全屏
<!--more-->
video标签还是比较常用的，在微信浏览器里面使用video标签，
会自动变成全屏，改成下面就好了，
起码可以在video标签之上加入其他元素。   
 ```<video id="videoID"webkit-playsinline="true" 
 x-webkit-airplay="true"  playsinline="true"
 x5-video-player-type="h5"
 x5-video-player-fullscreen="true"
 width="100%" height="100%" preload="auto"  
 poster="" src="">
 </video>```

还有个问题，在Android的微信里面，
就算加上了上面的属性，还会出现上下有黑边，不能全屏的问题。
解决办法：给video加上object-fit: fill;的style属性。
实现方法：   
```'<video id="videoplayer" controls="controls"'+ 
'tabindex="0" preload="metadata" style="width:100%; height: 100%; overflow: hidden;object-fit: fill;" '+
'poster="http://sun1.wxdg.sun0769.com/ImageResource/video.jpg" '+
'webkit-playsinline="true" x-webkit-airplay="true"  playsinline="true" '+
'x5-video-player-fullscreen="true"></video>'```