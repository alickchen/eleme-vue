<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content" >
    	<div class="overview">
    		<div class="overview-left">
    			<h1 class="score">{{seller.score}}</h1>
    			<div class="title">综合评分</div>
    			<div class="rank">高于周边商家{{seller.rankRate}}%</div>
    		</div>
    		<div class="overview-right">
    			<div class="score-wrapper">
    				<span class="title">服务态度</span>
    				<star :size="36" :score="seller.serviceScore"></star>
    				<span class="score">{{seller.serviceScore}}</span>
    			</div>
    			<div class="score-wrapper">
    				<span class="title">商品评分</span>
    				<star :size="36" :score="seller.foodScore"></star>
    				<span class="score">{{seller.foodScore}}</span>
    			</div>
    			<div class="delivery-wrapper">
    				<span class="title">送达时间</span>
    				<span class="delivery">{{seller.deliveryTime}}分钟</span>
    			</div>
    		</div>
    	</div>
    	<split></split>
    	<ratingselect :select-type="selectType" :only-content="onlyContent"  :ratings="ratings"  :eventHub="eventHub"></ratingselect>
    	<div class="rating-wrapper">
    		<ul>
    			<li v-for="rating in ratings" class="rating-item border-1px"  v-show="needShow(rating.rateType,rating.text)">
    				<div class="avatatr">
    					<img :src="rating.avatar" alt="">
    				</div>
    				<div class="content">
    					<h1 class="name">{{rating.username}}</h1>
    					<div class="star-wrapper">
    						<star :size="24" :score="rating.score"></star>
    						<span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
    					</div>
    					<p class="text">{{rating.text}}</p>
    					<div class="recommend" v-show="rating.recommend && rating.recommend.length > 0">
    						<span class="icon-thumb_up"></span>
    						<span v-for="item in rating.recommend" class="item">{{item}}</span>
    					</div>
    					<div class="time">{{rating.rateTime | formaDate}}</div>
    				</div>
    			</li>
    		</ul>
    	</div>
    </div>
  </div>
</template>

<script>
import star from '@/components/star/star';
import split from '@/components/split/split';
import ratingselect from '@/components/ratingselect/ratingselect';
import {formateDate} from '@/common/js/date';
import Vue from 'vue';
import BScroll from 'better-scroll';
const eventHub = new Vue();
const ALL = 2;
export default {
  name: 'ratings',
  props: {
  	seller: {
  		type: Object
  	}
  },
  components: {
           star,
           split,
           ratingselect
  },
  data() {
	return {
	showFlag: false,
	selectType: ALL,
	onlyContent: true,
	eventHub: eventHub,
	ratings: []
	};
   },
   created() {
  this.axios.get('/api/ratings')
  .then((response) => {
     response = response.data;
     if (response.errno === 0) {
        this.ratings = response.data;
        this.$nextTick(() => {
                     if (!this.scroll) {
                           this.scroll = new BScroll(this.$refs.ratings, {
                           	click: true
                           });
                     } else {
                     	this.scroll.refresh();
                     }
          	});
     }
  })
  .catch(function (error) {
    console.log(error);
  });

         this.eventHub.$on('ratingtype-select', (type) => {
         	this.selectType = type;
         	this.$nextTick(() => {
     	      	this.scroll.refresh();
         	    });
         });
         this.eventHub.$on('content-toggle', (b) => {
         	this.onlyContent = b;
         	this.$nextTick(() => {
         	      	this.scroll.refresh();
         	    });
         });
    },
    filters: {
    	formaDate(time) {
                let date = new Date(time);
                return formateDate(date, 'yyyy-MM-dd hh:mm');
    	}
    },
    methods: {
            needShow(type, text) {
                  if (this.onlyContent && !text) {
                  	return false;
                  }
                  if (this.selectType === ALL) {
                  	return true;
                  } else {
                  	return type === this.selectType;
                  }
          }
    }
};
</script>

<style lang="less" rel="stylesheet/less">
 @import url(../../common/less/mixin.less);
.ratings {
	position: absolute;
	top: 174px;
	bottom: 0;
	left: 0;
	width: 100%;
	overflow: hidden;
	.overview {
		display: flex;
		padding: 18px 0;
		.overview-left {
                               flex:0 0 130px;  
                               width: 130px;
                               padding: 6px 0;
                               border-right: 1px solid rgba(7,17,27,0.1);
                               text-align: center;
                               @media only screen and (max-width: 320px)  {
                               	          flex:0 0 120px;  
                               	          width: 120px;
                               	          padding: 5px 0;
                               }
                               .score {
                               	       margin-bottom: 6px;
                                       line-height: 28px;
                                       font-size: 24px;
                                       color: rgb(255,153,0)
                               }
                               .title {
                               	       margin-bottom: 8px; 
                               	       line-height: 12px;
                               	       font-size: 12px;
                               	       color: rgb(7,17,27);
                               }
                               .rank {
                                        color: rgb(147,153,159);
                                        font-size: 10px;
                                        line-height: 10px
                               }
		}
		.overview-right {
                               flex: 1;
                               padding: 6px 0 6px 24px;
                               @media only screen and (max-width: 320px)  {
                               	        padding-left: 5px;
                               }
                               .score-wrapper {
                               	        margin-bottom: 8px;
                               	        font-size: 0;
                               	        .title {
                               	        	    display: inline-block;
                                               font-size: 12px;
                                               color: rgb(7,17,27);
                                               vertical-align: top;
                                               line-height: 18px;
                               	        }
                               	        .star {
                               	        	     display: inline-block;
                               	        	     margin: 0 12px;
                               	        	     vertical-align: top;
                               	        }
                               	        .score {
                               	        	     display: inline-block;
                               	        	     vertical-align: top;
                               	        	     font-size: 12px;
                               	        	     color: rgb(255,153,0);
                               	        	     line-height: 18px;
                               	        }
                               }
                               .delivery-wrapper {
                               	         font-size: 0;
                               	         .title {
                                               font-size: 12px;
                                               color: rgb(7,17,27);
                                               line-height: 18px;
                               	         }
                               	         .delivery {
                               	         	      margin-left: 12px;
                               	         	      font-size: 12px;
                               	         	      color: rgb(147,153,159);
                               	         }
                               }

		}
	}
	.rating-wrapper {
		padding: 0 18px;
		.rating-item {
			display: flex;
			padding: 18px;
			.border-1px(rgba(7,17,27,0.1));
			.avatatr {
                                                flex: 0 0 28px;
                                                width: 28px;
                                                margin-right: 12px; 
                                                img {
                                                	width: 100%;
                                                	border-radius: 50%;
                                                }
			}
			.content {
			          position: relative;
			          flex:1;
			          .name {
                                                           line-height: 12px;
                                                           font-size: 10px;
                                                           color: rgb(7,17,27);
                                                           margin-bottom: 4px;
				}
			           .star-wrapper {
                                                           margin-bottom: 6px;
                                                           font-size: 0;
                                                           .star {
                                                              display: inline-block;
                                                              vertical-align: top;
                                                           }
                                                           .delivery {
                                                           	display: inline-block;
                                                                vertical-align: top;
                                                                color: rgb(147,153,159);
                                                                font-size: 10px;
                                                           }
				}
				.text {    
					margin-bottom: 8px;
					line-height: 18px;
					font-size: 12px;
					color: rgb(7,17,27);
				}
				.recommend {
					line-height: 16px;
					font-size: 0;
					.icon-thumb_up,.item{
                                                                         display: inline-block;
                                                                         margin: 0 8px 4px 0;
                                                                         font-size: 9px;
					}
					.icon-thumb_up {
					         color: rgb(0,160,220)
					}
					.item {
                                                                         border: 1px solid rgba(7,17,27,0.1);
                                                                         background: #fff;
                                                                         padding: 0 6px;
                                                                         color: rgb(147,153,159);
                                                                         border-radius: 1px;
					}
				}
				.time {
					position: absolute;
					top: 0;
					right: 0;
					font-size: 10px;
					line-height: 12px;
					color: rgb(147,153,159);
				}
			}
		}
	}
}

</style>
