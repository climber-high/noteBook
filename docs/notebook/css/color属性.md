## color属性

### 颜色的取值，分三种
```
1. 单词模式(优点容易记，缺点颜色精准性较弱) 
2. 16进制模式 （#FF0022）
3. rgba模式 RGB+Alpha(RGB取值范围0-255，A取值范围0-1)
4. hsla模式 (h:0~360° 代表颜色     s:0.0%~100.0% 代表灰度    L:0.0%~100.0% 代表亮度    a:0~1代表透明度)
```

###### 例：
```
background-color:red;
background-color:#FFFFFF ;(白色)
background-color:rgba(255,0,0,.5) ;(半透明的红色)
background-color: hsla(0,100%,50%,1);
```







