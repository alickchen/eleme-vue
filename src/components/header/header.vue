<template>
  <div class="header">
       <div class="content-wrapper">
       	<div class="avatar">
       	      <img width="64" height="64" :src="seller.avatar">	
       	</div>
       	<div class="content">
   		<div class="title">
   		    <span class="brand"></span>
   		    <span class="name">{{seller.name}}</span>
   		</div>
   		<div class="description">
   			{{seller.description}}/{{seller.deliveryTime}}分钟送达
   		</div>
   		<div v-if="seller.supports" class="supports">
   			<span class="icon" :class="classMap[seller.supports[0].type]"></span>
   			<span class="text">{{seller.supports[0].description}}</span>
   		</div>
       	</div>
       	<div v-if="seller.supports" class="supports-count">
       		<span class="count">{{seller.supports.length}}个</span>
       		<i class="icon-keyboard_arrow_right"></i>
       	</div>
       </div>
       <div class="bulletin-warpper">
       	<span class="bulletin-titile"></span><span class="bulletin-text">{{seller.bulletin}}</span>
           <i class="icon-keyboard_arrow_right"></i>
       </div>
       <div class="background">
       	 <img width="100%" height="100%" :src="seller.avatar">	
       </div>
  </div>
</template>

<script>
export default {
  name: 'header',
  props: {
        seller: {
              type: Object
          }
  },
  created() {
  this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  }
};
</script>

<style lang="less" rel="stylesheet/less">

// @import url(../../common/less/mixin.less);
.bg-image(@url) {
  background-image: url("@{url}@2x.png");
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
     background-image: url("@{url}@3x.png");
  }
};

.header {
	color: #fff;
	position: relative;
	background: rgba(7,17,27,0.5);
	.content-wrapper {
	    position: relative;
              padding: 24px 12px 18px 24px;
              font-size: 0;
              .avatar {
              	display: inline-block;
              	vertical-align: top;
              	img {
              	border-radius: 2px;
              	}
              }
              .content {
              	display: inline-block;
              	font-size: 14px;
              	margin-left: 16px;
                     .title  {
                        margin: 2px 0 8px 0;
                        .brand {
                        	vertical-align: top;
                        	width: 30px;
                        	height: 18px;
                        	display: inline-block;
                                .bg-image('brand');
                                background-size: 30px 18px;
                                background-repeat: no-repeat;
                        }
                         .name {
                            	margin-left: 6px;
                            	font-size: 16px;
                            	line-height: 18px;
                            	font-weight: bold;
                                }
                     }
                    .description {
                    	    margin-bottom: 10px;
                    	    line-height: 12px;
                    	    font-size: 12px;
                    }
                    .supports {
                    	.icon {
                    		display: inline-block;
                    		width: 12px;
                    		height: 12px;
                    		margin-left: 4px;
                    		background-size: 12px 12px;
                    		background-repeat: no-repeat;
                    		vertical-align: top;
                    		&.decrease {
                    			.bg-image("decrease_1");
                    		}
                    		&.discount {
                    			.bg-image("discount_1")
                    		}
                    		&.special {
                    			.bg-image("special_1")
                    		}
                    		&.invoice {
                    			.bg-image("invoice_1")
                    		}
                    	}
                    	.text {
                    		vertical-align: top;
                    		font-size: 10px;
                    		line-height: 12px;
                    	}
                    }
              }
              .supports-count {
              	position: absolute;
              	right: 12px;
              	bottom: 18px;
              	padding: 0 8px;
              	height: 24px;
              	line-height: 24px;
              	display: inline-block;
              	background: rgba(0,0,0,0.2);
              	border-radius: 14px;
              	.count {
              		font-size: 10px;
              	}
              	.icon-keyboard_arrow_right {
              		font-size: 10px;
              		line-height: 24px;
              		margin-left: 2px;
              	}
              }
	}
	.bulletin-warpper {
		height: 28px;
		line-height: 28px;
		padding: 0 22px 0 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		position: relative;
		background: rgba(7,17,27,0.2);
                     .bulletin-titile {
                         display: inline-block;
                         width: 22px;
                         height: 12px;
                         .bg-image('bulletin');
                         background-size: 22px 12px;
                         background-repeat: no-repeat;
                         vertical-align: top;
                         margin-top: 7px;
                     }
                     .bulletin-text {
                          font-size: 10px;
                          margin: 0 4px;
                          vertical-align: top;
                     }
                     .icon-keyboard_arrow_right {
                          position: absolute;
                          font-size: 10px;
                          right: 12px;
                          top: 8px;
                     }
	}
	.background {
		position: absolute;
		z-index: -1;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		filter: blur(10px);
	}
}
</style>

