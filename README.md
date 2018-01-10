### 百度地图自定义marker及点击事件
* 先贴代码

```
var mp = new BMap.Map("allmap");
mp.centerAndZoom(new BMap.Point(113.134026,23.035095), 11);
mp.enableScrollWheelZoom();
// 复杂的自定义覆盖物
function ComplexCustomOverlay(point, text,tid,bgcolor){
	this._point = point;
	this._text = text;
	this._tid = tid;
	this._bgcolor = bgcolor;
}
ComplexCustomOverlay.prototype = new BMap.Overlay();
ComplexCustomOverlay.prototype.initialize = function(map){
	this._map = map;
	var div = this._div = document.createElement("div");
	div.style.position = "absolute";
	div.style.zIndex = BMap.Overlay.getZIndex(this._point.lat);
	div.style.backgroundColor = this._bgcolor;
	div.style.border = "1px solid #BC3B3A";
	div.style.color = "white";
	div.style.height = "18px";
	div.style.padding = "2px";
	div.style.lineHeight = "18px";
	div.style.whiteSpace = "nowrap";
	div.style.MozUserSelect = "none";
	div.style.fontSize = "12px"
	var span = this._span = document.createElement("span");
	div.appendChild(span);
	span.appendChild(document.createTextNode(this._text)); 
	span.id=this._tid;     
	var that = this;

	var arrow = this._arrow = document.createElement("div");
	arrow.style.background = "url(http://map.baidu.com/fwmap/upload/r/map/fwmap/static/house/images/label.png) no-repeat";
	arrow.style.position = "absolute";
	arrow.style.width = "11px";
	arrow.style.height = "10px";
	arrow.style.top = "22px";
	arrow.style.left = "10px";
	arrow.style.overflow = "hidden";
	div.appendChild(arrow);
	if (this._clickFun) {
		div.addEventListener("touchstart",this._clickFun);
	}
	mp.getPanes().labelPane.appendChild(div);
	return div;
}
ComplexCustomOverlay.prototype.draw = function(){
	var map = this._map;
	var pixel = map.pointToOverlayPixel(this._point);
	this._div.style.left = pixel.x - parseInt(this._arrow.style.left) + "px";
	this._div.style.top  = pixel.y - 30 + "px";
}
ComplexCustomOverlay.prototype.addEventListener = function (event, fun) {
	if (event == 'touchstart') {
		this._clickFun = fun;
	}  
}
function cFun(e){
	var p = e.target;
	//alert("marker的位置是"+p.id);
	// 点击跳转
	location.href = 'xxx.html';
}
var myCompOverlay = new ComplexCustomOverlay(new BMap.Point(113.134026,23.035095), "佛山市15","1","#00FF00");
myCompOverlay.addEventListener('touchstart',cFun);
mp.addOverlay(myCompOverlay);
```

* 关键是添加touchstart事件，也可以用touchend事件代替，如果用click事件的话，在移动端会无效。
