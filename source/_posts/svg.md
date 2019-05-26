---
title: svg
date: 2019-04-25 17:47:42
tags:
---
####  拖拽
```html
<!DOCTYPE html>
<html>
<head>
	<title>Pad</title>
	<meta charset="utf-8">
	<style>
	</style>
</head>
<body>
	<svg id="pad" width="1000" height="500" style="border:1px solid black;"></svg>
</body>
<script type="text/javascript" src="snap.svg-min.js"></script>
<script type="text/javascript">
    const pad = Snap('#pad');

    const move = function(dx,dy){
    	this.attr({transform:this.data+(this.data?"T":"t")+[dx, dy]});
    };

    const before = function(){
    	this.data = this.transform().local;
    };

    const end = function(){};

    const rect=pad.paper.rect(100,100,100,200).attr({fill:"red"});
    
    rect.drag(move,before,end);
	
</script>
</html>
```
