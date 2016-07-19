# JQuery-RWD-One-Page-Scroll 滿版一頁式網頁

##載入jquery.js,mousewheel.js,touchSwipe.js

        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-mousewheel/3.1.13/jquery.mousewheel.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.touchswipe/1.6.18/jquery.touchSwipe.min.js"></script>
        
##HTML

        <ul class="nav">
        	<li><a href="#">1</a></li>
        	<li><a href="#">2</a></li>
        	<li><a href="#">3</a></li>
        	<li><a href="#">4</a></li>
        </ul>
        
        <div class="fullBox">
        	<div class="box1">1</div>
        	<div class="box2">2</div>
        	<div class="box3">3</div>
        	<div class="box4">4</div>
        </div>

##CSS

      html,body,.fullBox{ height:100%; margin: 0; padding: 0;}
      
      .nav{padding: 10px; position: fixed; top:0; right: 0; background: #fff; list-style: none}
      .fullBox>div{ height: 100%; font-size: 72px; text-align: center; }
      .box1{ background: #ccc; }
      .box2{ background: #aaa;}
      .box3{ background: #eee;}
      .box4{background: #ddd; }

##JS

        $(function(){
        	var $window = $(window);
        	var $body = $('body,html');
        	var $fullBox = $(".fullBox");
        	var $fullBoxDiv = $fullBox.children('div');
        	var _maxIndex = $fullBoxDiv.length-1;
        	var _index = 0;
        	var wheeling,scrolling,positionRevise;
        
        
        	$fullBoxDiv.each(function() {
        		if( $window.scrollTop() > $(this).offset().top){
        			_index++;
        		}
        	});
        
        	function goDiv(_index){
        		$body.animate({scrollTop: $fullBoxDiv.eq(_index).offset().top}, 300); 
        	}
        
        
        	$('.nav a').click(function() {
        		_index = $(this).parent("li").index();
        		goDiv(_index);
        		return false;
        	});
        
        
        	$fullBox.mousewheel(function(event, delta){ // delta 若是負值是滾輪往下滾；反之則是滾輪往上滾
        		clearTimeout(scrolling);
        		clearTimeout(positionRevise);
        		clearTimeout(wheeling);
        	  	wheeling = setTimeout(function() {  	
        			if(delta < 0){
        				if(_index < _maxIndex ){
        					_index++;
        				}	
        			}else{
        				if(_index > 0){
        					_index--;
        				}	
        			}
        			goDiv(_index);
        	  	}, 100);
        		return false;
        	});
        
        	var lastScrollTop = 0;
        	$window.scroll(function(event){
        	    var st = $(this).scrollTop();
        
        	    clearTimeout(scrolling);
        	   	scrolling = setTimeout(function() { 
        		    if (st > lastScrollTop){			//往下
        		        if (st != lastScrollTop && _index < _maxIndex && $window.scrollTop() >= $fullBoxDiv.eq(_index+1).offset().top){
        					_index++;
        		        } 
        		   	} else {							//往上
        		      	if(_index > 0 && $window.scrollTop() <= $fullBoxDiv.eq(_index-1).offset().top){
        					_index--;
        				}	
        		   	}
        		   	lastScrollTop = st;
        		}, 100);    	
        
        	   	clearTimeout(positionRevise);
        	   	positionRevise = setTimeout(function() { 
        
        		    if( $window.scrollTop() > $fullBoxDiv.eq(_index).offset().top + $window.height()/2 ){
        				_index++;
        				goDiv(_index);
        		    }else if( $window.scrollTop() < $fullBoxDiv.eq(_index).offset().top - $window.height()/2){
        				_index--;
        				goDiv(_index);
        		    }else{
        		   		goDiv(_index);
        		    }
        		}, 1500); 
        	  	  
        	});
        
        
        	if(isMobile()){
        
        		$("body").css("overflow","hidden");
        
        		$fullBox.swipe( {
        		  swipeUp:function() {
        		    clearTimeout(scrolling);
        			clearTimeout(positionRevise);
        			if(_index < _maxIndex ){
        				_index++;
        				goDiv(_index);
        			}
        		  },
        		  swipeDown:function() {
        		    clearTimeout(scrolling);
        			clearTimeout(positionRevise);
        			if(_index > 0){
        				_index--;
        				goDiv(_index);
        			}		
        		
        		  },
        		  threshold:50
        		});
        
        	
        	}
        	
        });
        
        
        function isMobile() {
        	if (/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
        		return true;
        	} else { 
        		return false; 
        	}
        }




