<template>
  <div class="star" :class="startype">
  	<span v-for="item in itemClass" :class="item" class="star-item"></span>
    
  </div>
</template>

<script>
const length=5;
const cls_on='on'
const cls_half='half'
const cls_off='off'

export default {
  	 props:{
  	 	size:{
  	 		type:Number
  	 	},
  	 	score:{
  	 		type:Number
  	 	}
  	 },
  	 computed:{
  	 	startype() {
  	 		return 'star-'+this.size; 
  	 	},
      itemClass() {
        let result=[];        
        let half=this.score%1!==0;
        let integer= Math.floor(this.score);
        for (let i = 0; i < integer; i++) {
          result.push(cls_on);
        }
        if(half){
          result.push(cls_half);
        }
        while(result.length<length){
          result.push(cls_off)
        }
        
        return result;
      }
  	 }
  }
</script>

<style>
.star .star-item{
  display: inline-block;
  background-repeat: no-repeat;
}
.star .star-item:last-child{
  margin-right: 0;
}

.star-48 .star-item{
	width:20px;
	height:20px;
	margin-right: 12px;
	background-size: 20px 20px;
}

.star-48 .on{
	background-image: url(star48_on@2x.png);
}
.star-48 .half{
  background-image: url(star48_half@2x.png);
}
.star-48 .off{
  background-image: url(star48_off@2x.png);
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-48 .on{
    background-image: url(star48_on@3x.png)
  }
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-48 .half{
    background-image: url(star48_half@3x.png)
  }
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-48 .off{
    background-image: url(star48_off@3x.png)
  }
}



.star-36 .star-item{
  width:15px;
  height:15px;
  margin-right:6px;
  background-size: 15px 15px;
}

.star-36 .on{
  background-image: url(star36_on@2x.png);
}
.star-36 .half{
  background-image: url(star36_half@2x.png);
}
.star-36 .off{
  background-image: url(star36_off@2x.png);
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-36 .on{
    background-image: url(star36_on@3x.png)
  }
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-36 .half{
    background-image: url(star36_half@3x.png)
  }
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-36 .off{
    background-image: url(star36_off@3x.png)
  }
}


.star-24 .star-item{
  width:10px;
  height:10px;
  margin-right: 3px;
  background-size: 10px 10px;
}

.star-24 .on{
  background-image: url(star24_on@2x.png);
}
.star-24 .half{
  background-image: url(star24_half@2x.png);
}
.star-24 .off{
  background-image: url(star24_off@2x.png);
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-24 .on{
    background-image: url(star24_on@3x.png)
  }
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-24 .half{
    background-image: url(star24_half@3x.png)
  }
}
@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
  .star-24 .off{
    background-image: url(star24_off@3x.png)
  }
}
</style>