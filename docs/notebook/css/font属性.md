## font属性
```
font-size: 24px;         字体大小
font-size: 200%;         除了绝对值，也可以用百分比，基于父元素的字体大小进行百分比计算
font-weight: bold;       字体的粗细，通常加粗就写bold,不加粗就写normal
font-style: italic;      字体的倾斜，通常斜体就写italic,不需要斜体就写
font-style: oblique;     字体的倾斜
font-variant: small-caps;小型的大写字母，只能应用于内容中小写字母，使用频率不高
```
```
网站字体，主要分为衬线和非衬线
通常网站主流使用非衬线字体（"san-serif"）
		      英文                                中文
		win | Arial                           |"Microsoft Yahei"
		mac |"Helvetica Neue",Helvetica       |"PingFang SC", "Hiragino Sans GB","Heiti SC"
```

### 注意
```
font-family:"Helvetica Neue",Helvetica,Arial,"PingFang SC", "Hiragino Sans GB","Heiti SC","san-serif";

如果以上中文字体都没有，我们就会给个保底选择，叫非衬线字体（"san-serif"）
写法上，优先写mac系统，再写win系统的英文；再一起写中文的，优先写英文字体，再写中文字体
		
浏览器在应用的过程中，会优先判断英文字体，再判断中文字体，多个字体
要用空格隔开，如果字体名字带有空格都需要用引号包起来。
```

```
以上属性都是单独的写法，我们可以用font进行整合缩合
```
###### 例：
```
font:bold italic small-caps 24px/32px "Helvetica Neue",Helvetica,Arial,
"PingFang SC", "Hiragino Sans GB","Heiti SC","san-serif";
```

### 注意：
```
实际使用，还是建议大家根据需求写独立属性，更容易记忆和理解 
```
