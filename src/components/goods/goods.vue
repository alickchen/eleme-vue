<template>
  <div class="goods">
    <div class="menu-wrapper"  ref="menuwrapper">
    	<ul>
    		<li v-for="(item,index) in goods" class='menu-item' :class="{current: currentIndex === index}" @click="selectMenu(index,$event)">
    			<span class="text border-1px">
    				<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
    			</span>
    		</li>
    	</ul>
    </div>
    <div class="foods-wrapper" ref="foodswrapper">
    	<ul>
    		<li v-for="item in goods" class="food-list food-list-hook">
    			<h1 class="title">{{item.name}}</h1>
    			<ul>
    				<li v-for="food in item.foods" class="food-item">
    					<div class="icon">
    						<img width="57" height="57" :src="food.icon">
    					</div>
    					<div class="content">
    						<h2 class="name">{{food.name}}</h2>
    						<p class="desc">{{food.description}}</p>
    						<div class="extra">
    							<span class="count">月售{{food.sellCount}}份</span>
    							<span>好评率{{food.rating}}%</span>
    						</div>
    						<div class="price">
    							<span class="now">￥{{food.price}}</span><span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
    						</div>
    						<div class="cartcontrol-wrapper">
    							<cartcontrol  :food="food" :eventHub="eventHub"></cartcontrol>
    						</div>
    					</div>
    				</li>
    			</ul>
    		</li>
    	</ul>
    </div>
    <shopcart  :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :select-foods="selectFoods" :eventHub="eventHub"></shopcart>
  </div>
</template>

<script>
import Vue from 'vue';
import BScroll from 'better-scroll';
import shopcart from '@/components/shopcart/shopcart';
import cartcontrol from '@/components/cartcontrol/cartcontrol';

const eventHub = new Vue();

export default {
  name: 'goods',
  props: {
  	seller: {
  		type: Object
  	}
  },
  components: {
  	shopcart,
  	cartcontrol
  },
  data () {
  	return {
  		goods: [],
  		listHeight: [],
  		scrollY: 0,
           eventHub: eventHub
  	};
  },
  computed: {
     currentIndex() {
          for (let i = 0; i < this.listHeight.length; i++) {
          	let height1 = this.listHeight[i];
          	let height2 = this.listHeight[i + 1];
          	if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          		return i;
          	}
          }
          return 0;
     },
     selectFoods() {
             let foods = [];
             this.goods.forEach((good) => {
             	good.foods.forEach((food) => {
                         if (food.count) {
                               foods.push(food);
                            }
             	});
             });
             return foods;
     }
  },
    created () {
  this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];

  this.axios.get('/api/goods')
  .then((response) => {
     response = response.data;
     if (response.errno === 0) {
        this.goods = response.data;
        this.$nextTick(() => {
                   this._initScroll();
                   this._calculateHeight();
        });
     }
  })
  .catch(function (error) {
    console.log(error);
  });
  },
  methods: {
  	_initScroll() {
  		this.menuScroll = new BScroll(this.$refs.menuwrapper, {
  			click: true
  		});

  		this.foodsScroll = new BScroll(this.$refs.foodswrapper, {
  			click: true,
  			probeType: 3
  		});
  		this.foodsScroll.on('scroll', (pos) => {
  			this.scrollY = Math.abs(Math.round(pos.y));
  		});
  	},
  	_calculateHeight() {
  	           let foodlist = this.$refs.foodswrapper.getElementsByClassName('food-list-hook');
  		let height = 0;
  		this.listHeight.push(height);
  		for (let i = 0; i < foodlist.length; i++) {
  			let item = foodlist[i];
  			height += item.clientHeight;
  			this.listHeight.push(height);
  		}
  	},
  	selectMenu (index, event) {
	   if (!event._constructed) {
		   return;
	   };
              let foodlist = this.$refs.foodswrapper.getElementsByClassName('food-list-hook');
              let el = foodlist[index];
              this.foodsScroll.scrollToElement(el, 300);
  	}
  }
};
</script>

<style lang="less" rel="stylesheet/less">
 @import url(../../common/less/mixin.less);
.bg-image(@url) {
  background-image: url("@{url}@2x.png");
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3) {
     background-image: url("@{url}@3x.png");
  }
};
.goods {
	position: absolute;
	width: 100%;
	top: 174px;
	bottom: 64px;
	display: flex;
	overflow: hidden;
	.menu-wrapper {
                 flex: 0 0 80px;
                 width: 80px;
                 background: #f3f5f7;
                 .menu-item {
                 	display: table;
                 	height: 54px;
                 	width: 56px;
                 	line-height: 14px;
                 	padding: 0 12px;
                 	.icon {
                    		display: inline-block;
                    		width: 12px;
                    		height: 12px;
                    		margin-left: 2px;
                    		background-size: 12px 12px;
                    		background-repeat: no-repeat;
                    		vertical-align: top;
                    		&.decrease {
                    			.bg-image("decrease_3");
                    		}
                    		&.discount {
                    			.bg-image("discount_3")
                    		}
                    		&.special {
                    			.bg-image("special_3")
                    		}
                    		&.invoice {
                    			.bg-image("invoice_3")
                    		}
            	          &.guarantee {
	             	          .bg-image("guarantee_3")
            	          }
                    	}
                    	.text {
                    		display: table-cell;
                    		width: 54px;
                    		vertical-align: middle;
                                font-size: 12px;
                                text-align: center;
                                .border-1px(rgba(7,17,27,0.1));
                    	}
                    	&.current {
                 		position: relative;
                 		z-index: 10;
                 		background: #fff;
                 		font-weight: 700;
                 		margin-top: -1px;
                 		.text {
                 			&:after {
                 			border: none;
                 			}
                 		}

                 	}
                 }
	}
	.foods-wrapper {
                 flex: 1;
                 .title {
                 	padding-left: 14px;
                 	height: 26px;
                 	line-height: 26px;
                 	border-left: 2px solid #d9dde1;
                 	font-size: 12px;
                 	color: rgb(147,159,159);
                 	background: #f3f5f7;
                 }
                 .food-item {
                      display: flex;
                      margin: 18px;
                      .border-1px(rgba(7,17,27,0.1));
                      padding-bottom: 18px;
                      &:last-child {
                      	.border-none(); 
                      	margin-bottom: 0;
                      }
                      .icon {
                                flex: 0 0 57px;
                                margin-right: 10px;
                      }
                      .content {
                      	flex:1;
                      	.name {
                      		margin: 2px 0 8px 0 ;
                      		height: 14px;
                      		line-height: 14px;
                      		font-size: 14px;
                      		color: rgb(7,17,27);
                      	}
                      	.desc ,.extra{
                      		margin-bottom: 8px;
                      		line-height: 10px;
                      		font-size: 10px;
                      		color: rgb(147,153,159);
                      	}
                      	.desc {
                      		line-height: 12px;
                      	}
                      	.extra {
                                           line-height: 10px;
                                           .count {
                                           	margin-right: 12px;
                                           }

                      	}
                      	.price {
                                           font-weight: 700;
                                           line-height: 24px;
                                           .now {
                                           	margin-right: 8px;
                                           	font-size: 14px;
                                           	color: rgb(240,20,20);
                                           }
                                           .old {
                                           	color: rgb(147,153,159);
                                           	text-decoration: line-through;
                                           	font-size: 10px;
                                           }
                                }
                                .cartcontrol-wrapper {
                                	position: absolute;
                                	right: 0;
                                	bottom: 12px;
                                }
                       }
                 }
	}
}
</style>
