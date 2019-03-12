<template>
	<div id="intro">
		<!--<button @click="show = !show">Toggle show</button>-->
		<button id="init" v-show="showInit" @click="init()">INICIAR</button>
		<div id="textContainer">
			<div id="centerDiv1">
				<transition name="zoom" v-on:after-enter="startSecondText()">
					<p id="text1" v-if="showTitle" ref="animatedText">LOREM<br/>IPSUM</p>
				</transition>
			</div>
		</div>

		<canvas ref="canvas" id="canvas"></canvas>

		<div class="center">
			<transition name="p" v-on:after-enter="animationEnds()">
				<div  v-if="showHistory" class="perspectiva">
				LOREM IPSUM DOLOR SIT AMET, CONSECTETUR ADIPISCING ELIT, SED DO EIUSMOD TEMPOR INCIDIDUNT UT LABORE ET DOLORE MAGNA ALIQUA. UT ENIM AD MINIM VENIAM, QUIS NOSTRUD EXERCITATION ULLAMCO LABORIS NISI UT ALIQUIP EX EA COMMODO CONSEQUAT. DUIS AUTE IRURE DOLOR IN REPREHENDERIT IN VOLUPTATE VELIT ESSE CILLUM DOLORE EU FUGIAT NULLA PARIATUR. EXCEPTEUR SINT OCCAECAT CUPIDATAT NON PROIDENT, SUNT IN CULPA QUI OFFICIA DESERUNT MOLLIT ANIM ID EST
				</div>
			</transition>
		</div>

	</div>
</template>

<script>
	export default({
		name:'Intro',
		data(){
			return {
				showInit: true,
				showTitle: false,
				showHistory: false,

			}
		},
		mounted(){
  			this.startAnimation();
		},
		methods:{
			init(){
				this.showInit = false;
				this.showTitle = true;
			},
			animationEnds(){
				console.log('animationEnds');
				this.showInit = true;
				this.showTitle =  false;
				this.showHistory = false;
			},
			startSecondText(){
				console.log('Iniciando segundo texto');
				var elm = this.$refs['animatedText'];
				console.log(elm);
				/*var newElm = elm.cloneNode(true);
				elm.parentNode.replaceChild(newElm,elm);*/
				this.showTitle = false;
				this.showHistory = true;
			},
			startAnimation(){
				var c = this.$refs['canvas'];
				var ctx = c.getContext('2d');
	  			ctx.canvas.width  = window.innerWidth;
	  			ctx.canvas.height = window.innerHeight;
	  			ctx.strokeStyle = '#FFFFFF';

	  			this.starsGenerator(ctx,20);
	  			//this.movingText(ctx);
			},
			starsGenerator(ctx, segments){
				var relationXY = ctx.canvas.width / ctx.canvas.height;
	  			var segmentsY = segments;
	  			var segmentSizeY = ctx.canvas.height / segmentsY;
	  			var segmentSizeX = segmentSizeY * relationXY;
	  			var segmentsX = ctx.canvas.width / segmentSizeX;

	  			segmentSizeX = Math.floor(segmentSizeX);
	  			segmentSizeY = Math.floor(segmentSizeY);

				var limInfX, limSupX = 0;
	  			var limInfY, limSupY = 0;
	 			var difX = 0;
	 			var difY = 0;
	 			var pointRandX = 0;
	 			var pointRandY = 0;

	  			for(let cont = 0; cont < segmentsY; cont++){
	  				limInfY = segmentSizeY * cont + 1;
		  			limSupY = limInfY + segmentSizeY;
		  			difY = limSupY - limInfY;
	  				for(let cont2 = 0; cont2 < segmentsX; cont2++){
	  					limInfX = segmentSizeX * cont2 + 1;
		  				limSupX = limInfX + segmentSizeX;
		  				difX = limSupX - limInfX;

		  				pointRandX = (Math.random() * difX) + limInfX;
		  				pointRandY = (Math.random() * difY) + limInfY;

		  				ctx.lineWidth = (Math.random() * 1.8) + 0.1;
		  				ctx.beginPath();
				  		ctx.moveTo(pointRandX,pointRandY);
				  		ctx.lineTo(pointRandX+1,pointRandY);
				  		ctx.stroke();
	  				}
	  			}
			},
			movingText(ctx){
				var text = 'Lorem Ipsum';
				ctx.strokeStyle = '#FFCE46';
				ctx.font = "180px Arial";
				ctx.strokeText("Hello World", 0, 0);
			}
		}
	});
</script>

<style>

	#init{
		position: absolute;
		z-index: 100;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		font-size: 3rem;
		font-family: Impact;
		background-color: #FFCE46;
		width: 30%;
		height: 20%;
		border-radius: 10px;
		border-color: white;
		cursor: pointer;
	}

	.center{
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		/*background-color: red;*/
	}

	.perspectiva{
		position: relative;
		text-align: center;
		/*background-color: skyblue;*/
	    height: 400px;
	    top:200px;
	    transform: perspective(50px) rotateX(10deg) translateY(290px);
	    font-family: Impact;
		/*color: yellow;*/
		font-size: 4rem;
		color: #FFCE46;
		user-select: none;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
	}

	.p-enter-active {
	  animation: p-out 45s linear reverse forwards;
	}
	@keyframes p-out{
		0% {
	    	transform: perspective(50px) rotateX(10deg)  translateY(-4000px);
	    },
	    100% {
	    	transform: perspective(50px) rotateX(10deg) translateY(290px);
	    }
	}

	.zoom-enter-active {
	  animation: zoom-out 15s forwards;
	}
	@keyframes zoom-out{
		0% {
	    	-webkit-text-stroke: 10px #00000000;
	    	transform: scale(2,2);
	    },
		20% {
	        transform: scale(1,1);
	    }
	    100% {
	    	transform: scale(.1,.1);
	    }
	}

	#intro{
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
	#canvas{
		width: 100%;
		height: 100%;
		background-color: black;
	}
	#textContainer{
		position: absolute;
		text-align: center;
		width: 100%;
		height: 100%;
		margin: 0;
		perspective: 1000px;
	}

	#centerDiv1{
		position: absolute;
		top: 50%;
		left: 50%;
		width: 100%;
		/*background-color: red;*/
		transform: translate(-50%,-50%);
	}

	#text1{
		font-family: Impact;
		/*color: yellow;*/
		font-size: 20rem;
		color: #00000000;
		-webkit-text-stroke: 10px #FFCE46;
		margin: 0;
		padding: 0;
		line-height: 0.85;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
	}

	#centerDiv2{
		position: absolute;
		left: 50%;
		width: 80%;
		/*background-color: red;*/
		transform: translateX(-50%);
		perspective: 1000px;
	}

	#text2{
		width: 100%;
		font-family: Impact;
		/*color: yellow;*/
		font-size: 6rem;
		color: #FFCE46;
		margin: 0;
		padding: 0;
		line-height: 1;
		user-select: none;
		text-align: center;
		transform: rotateX(50deg);
		overflow-wrap: break-word;

	}

	@media only screen and (max-width: 600px) {

	}

	@media only screen and (min-width: 600px) {

	} 

	@media only screen and (min-width: 768px) {

	}

	@media only screen and (min-width: 992px) {

	}

	@media only screen and (min-width: 1200px) {

	}
</style>