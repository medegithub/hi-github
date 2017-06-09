<template>
	<div id="paint">
		<div class="paint_model"></div>
        <canvas id="myCanvas"></canvas>
        <label for="fontWidth">画笔宽度：<input type="text" id="fontWidth"></label>
        <label for=""></label>
    </div>
</template>

<style>
	*{margin:0; padding: 0}
	.paint_model{
		position: absolute;
		width: 100%;
		height: 100%;
		background: url('../../public/static/bear_t1.png') center no-repeat;
		background-size: 3.5rem 5rem;
	}
	#myCanvas{
		position: absolute;
		top:0;
		left: 0;
		z-index: 5;
		background: transparent;
	}
	.point_box{
		pointer-events: none;
	}
</style>

<script>
	export default{
		data(){
			return {

			}
		},
		mounted(){
			/*var img = $('#paint img');
			img.on('load',function(){
				var $this = $(this);
				var imgWidth = $this[0].naturalWidth;
				var imgHg = $this[0].naturalHeight;
				var width = $this.width();
				var wid = $(window).width();
				alert(imgWidth + ', ' + width + ',' + wid + ', ' + imgHg);
			});*/
			var arrPoint = [];
	        var canvas = document.getElementById('myCanvas');
	        var ctx = canvas.getContext('2d');
	        
	        var wid = window.innerWidth;
	        var hg = window.innerHeight;

	        canvas.width = wid;
	        canvas.height = hg;

	        ctx.lineWidth = 3;
	        // ctx.lineJoin = 'round';
	        ctx.strokeStyle = '#ffffff';
	        ctx.clearRect(0,0,wid,hg);

	        var count = 0;

	        canvas.addEventListener('touchstart',function(e){
	            e = event || window.event;
	            var touch = {};
	            touch.x = e.touches[0].clientX;
	            touch.y = e.touches[0].clientY;
	            if(arrPoint.length){
	            	touch.index = ++count;
	            	for(var i = 0; i < arrPoint.length; i++){
		            	var dirX = Math.abs(touch.x - arrPoint[i].x);
		            	var dirY = Math.abs(touch.y - arrPoint[i].y);
		            	if(dirX <= 12 && dirY <= 12){
		            		ctx.moveTo(arrPoint[i].x,arrPoint[i].y);
		            		touch.repeat = true;
		            	}else{
		            		touch.repeat = false;
		            		touch.line = [];
		            		arrPoint.push(touch);
			            	var html = '<div class="point_box" style="width:12px; height:12px; border-radius:6px; background:#fff; position:absolute; z-index:9; top:'+touch.y+'px;left:'+touch.x+'px;margin-top:-6px; margin-left:-6px"></div>';
			            	$('body').append(html);
			            	ctx.moveTo(touch.x,touch.y);
		            	}
		            } 
	            }else{
	            	touch.index = count;
	            	touch.repeat = false;
            		if(arrPoint.length){
            			touch.index = ++count;
            		}else{
            			touch.index = count;
            		}
            		touch.line = [];
            		arrPoint.push(touch);
	            	var html = '<div class="point_box" style="width:12px; height:12px; border-radius:6px; background:#fff; position:absolute; z-index:9; top:'+touch.y+'px;left:'+touch.x+'px;margin-top:-6px; margin-left:-6px"></div>';
	            	$('body').append(html);
	            	ctx.moveTo(touch.x,touch.y);
	            }    
	        },false);
	        
	        canvas.addEventListener('touchend',function(e){
	            e = event || window.event;
	            var touch = {};
	            var repeatPoint = {};
	            touch.x = e.changedTouches[0].clientX;
	            touch.y = e.changedTouches[0].clientY;
	            for(var i = 0; i < arrPoint.length; i++){
	            	var dirX = Math.abs(touch.x - arrPoint[i].x);
	            	var dirY = Math.abs(touch.y - arrPoint[i].y);
	            	if(arrPoint[i].repeat){
	            		repeatPoint = arrPoint[i];
	            	}
	            	if(dirX <= 12 && dirY <= 12){
	            		ctx.lineTo(arrPoint[i].x,arrPoint[i].y);
	            		ctx.stroke();
	            		if(repeatPoint.repeat){
	            			repeatPoint.line.push(arrPoint[i].index);
	            			arrPoint[i].line.push(repeatPoint.index);
	            		}else{
	            			arrPoint[arrPoint.length - 1].line.push(arrPoint[i].index);
	            			arrPoint[i].line.push(arrPoint[arrPoint.length - 1].index);
	            		}
	            	}else{
	            		touch.index = ++count;
	            		touch.line = [];
	            		arrPoint.push(touch);
			            var html = '<div class="point_box" style="width:12px; height:12px; border-radius:6px; background:#fff; position:absolute; z-index:9; top:'+touch.y+'px;left:'+touch.x+'px;margin-top:-6px; margin-left:-6px"></div>';
			            	$('body').append(html);
			            ctx.lineTo(touch.x,touch.y);
			            ctx.stroke();
			            if(repeatPoint.repeat){
	            			repeatPoint.line.push(touch.index);
	            			touch.line.push(repeatPoint.index);
	            		}else{
	            			arrPoint[arrPoint.length - 2].line.push(touch.index);
	            			touch.line.push(arrPoint[arrPoint.length - 2].index);
	            		}
	            	}
	            }
	    	},false);
		}
	}
</script>