# amp-bind 
## 以下是学习amp-bind的学习笔记 
1. 在header中引入amp-bind组件 
 
        <script async custom-element="amp-bind" src="https://cdn.ampproject.org/v0/amp-bind-0.1.js"></script> 

2. `amp-bind`要生效需要把元素绑定到一个隐式的JSON"state",这个state可以被一个或多个<amp-state>组件初始化，eg:
  
	    <amp-state id="allAnimals">
	      <script type="application/json">
	        {
	          "currentAnimal": "dog",
	          "dog": {
	            "imageUrl": "/img/Border_Collie.jpg",
	            "videoUrl": "/video/dog-video.mp4",
	            "style": "greenBackground",
	            "iframeUrl": "https://player.vimeo.com/video/183849543"
	          },
	          "cat": {
	            "imageUrl": "/img/cat-looking-up-300x200.jpg",
	            "videoUrl": "/video/cat-video.mp4",
	            "style": "redBackground",
	            "iframeUrl": "https://player.vimeo.com/video/185199565"
	          }
	        }
	      </script>
	    </amp-state>  
3. 在绑定表达式里引用隐式状态是以<amp-state>元素的id开始，使用"."或"[]",e.g. 
  
		allAnimals.dog.imageUrl 或 allAnimals['cat']['imageUrl']
4. 数据绑定   
	Three types of element state can be bound:  
	* text for `textContent`
	* class for `CSS classes`
	* `Element-specific attributes`, for example, the `src` attribute for amp-img or `amp-video`.  
	
	To create a binding, create a new attribute on an element with the syntax:  

		[attribute]="expression"  
  

5. Binding Text  
The text of this `<p>` will change when the `currentAnimal` variable changes. `currentAnimal` is an uninitialized implicit state variable, which will be set when a state change is triggered (see below). A state variable default can be specified using `amp-state`.  
  
		<amp-state id="allAnimals" class="i-amphtml-element i-amphtml-layout-container" aria-hidden="true" style="display: none;">
		    <script type="application/json">
		    {
		          "currentAnimal": "dog",
		          "dog": {
		            "imageUrl": "/img/Border_Collie.jpg",
		            "videoUrl": "/video/dog-video.mp4",
		            "style": "greenBackground",
		            "iframeUrl": "https://player.vimeo.com/video/183849543"
		          },
		          "cat": {
		            "imageUrl": "/img/cat-looking-up-300x200.jpg",
		            "videoUrl": "/video/cat-video.mp4",
		            "style": "redBackground",
		            "iframeUrl": "https://player.vimeo.com/video/185199565"
		          }
		        }
		    </script>
		</amp-state>
		<p [text]="'This is a ' + allAnimals.currentAnimal + '.'">This is a dog.</p>  
显示为：This is a dog.  
6. Binding Styles  
Styling can be changed by applying CSS classes, which will override all the element's style classes. In the next example, allAnimals is the id of the amp-state component above and we use bracket（中括号） notation because currentAnimal is a variable(属性）.  
  
		<p [class]="allAnimals[allAnimals.currentAnimal].style"class="greenBackground">Each animal has a different background color.</p>  
7. Binding Attribute Values  
Various AMP components support binding attribute values with `amp-bind`:
	* `amp-img`
	* `amp-video`
	* `amp-iframe`  
	  
	`amp-img`URLs can be changed  


		<amp-img id="amp-img"
		  src="/img/Border_Collie.jpg"
		  layout="responsive"
		  width="300"
		  height="200"
		  [src]="allAnimals[allAnimals.currentAnimal].imageUrl">
		</amp-img>
The `src` URL for videos embedded with `amp-video` can be changed. At the moment, among all the AMP video components, `amp-video` and `amp-youtube` support `amp-bind`.  

		<amp-video id="amp-video"
		  src="/video/dog-video.mp4"
		  layout="responsive"
		  [src]="allAnimals[allAnimals.currentAnimal].videoUrl"
		  width="300"
		  height="170"
		  autoplay
		  controls></amp-video>
The `src` for iframes embedded with `amp-iframe` can be changed.

		<amp-iframe id="amp-iframe"
		  title="Cute dog video"
		  width="500"
		  height="281"
		  layout="responsive"
		  sandbox="allow-scripts allow-same-origin allow-popups"
		  allowfullscreen
		  frameborder="0"
		  src="https://player.vimeo.com/video/183849543"
		  [src]="allAnimals[allAnimals.currentAnimal].iframeUrl">
		</amp-iframe>  
8. Triggering updates  
Data bindings are re-evaluated when implicit state changes. Implicit state can be updated via the `AMP.setState` action.
	* `AMP.setState` merges new state into existing implicit state
	* Existing state can be overwritten, including state initialized by `amp-state` components  

			<button class="ampstart-btn caps m1"
			  on="tap:AMP.setState({allAnimals: {currentAnimal: 'cat'}})">
			  Set to Cat
			</button>



