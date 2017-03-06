<template>
  <div class="star"  :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>

<script>
export default {
  name: 'ratings',
  props: {
  	size: {
  		type: Number
  	},
  	score: {
  		type: Number
  	}
  },
  computed: {
  	starType () {
  		return "star" + this.size;
  	}
  }
};
</script >


<style  lang="less" rel="stylesheet/less" >
.bg-image(@url) {
  background-image: url("@{url}@2x.png");
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
     background-image: url("@{url}@3x.png");
  }
};
.star {
    font-size: 0;
    .star-item {
    	display: inline-block;
    	background-repeat: no-repeat;
    	&.star-48 {
	width: 20px;
    	height: 20px;
    	margin-right: 22px;
    	background
    	}
    	
    }
}


</style>
