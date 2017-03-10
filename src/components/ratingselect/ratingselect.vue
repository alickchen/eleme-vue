<template>
  <div class="ratingselcet">
  	<div class="rating-type border-1px">
  		<span class="block positive" :class="{'active': selectType === 2}" @click="select(2,$event)">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
  		<span class="block positive" :class="{'active': selectType === 0}" @click="select(0,$event)">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
  		<span class="block negative" :class="{'active': selectType === 1}" @click="select(1,$event)">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
  	</div>
  	<div @click="toggleContent($event)" class="switch" :class="{'on':onlyContent}">
  		<span class="icon-check_circle"></span>
  		<span class="text">只看有内容的评价</span>
  	</div>
  </div>
</template>

<script>
     const POSITIVE = 0;
     const NEGATIVE = 1;
     const ALL = 2;

export default {
	props: {
		ratings: {
			type: Array,
			default() {
				return [];
			}
		},
		selectType: {
			type: Number,
			default: ALL
		},
		onlyContent: {
			type: Boolean,
			default: false
		},
		desc: {
			type: Object,
			default() {
				return {
					all: '全部',
					positive: '满意',
					negative: '不满意'
				};
			}
		},
		eventHub: {
       		type: Object
       	              }
	},
	methods: {
		select(type, event) {
                         if (!event._constructed) {
		         return;
	                }
	               this.eventHub.$emit('ratingtype-select', type);
		},
		toggleContent() {
                         if (!event._constructed) {
		         return;
	                }
	               this.eventHub.$emit('content-toggle', !this.onlyContent);
		}
	},
	computed: {
		positives() {
                             return this.ratings.filter((rating) => {
                             	return rating.rateType === POSITIVE;
                             });
		},
		negatives() {
                             return this.ratings.filter((rating) => {
                             	return rating.rateType === NEGATIVE;
                             });
		}

	}
};
</script>

<style lang="less" rel="stylesheet/less">
 @import url(../../common/less/mixin.less);
.ratingselcet {
	.rating-type {
                   padding: 18px 0;
                   margin: 0 18px;
                   .border-1px(rgba(7,17,27,0.1));
                   font-size: 0;
                   .block {
                         display: inline-block;
                         padding: 8px 12px;
                         margin-right: 8px;
                         line-height: 16px;
                         border-radius: 1px;
                         font-size: 12px;
                         color: rgb(77,85,93);
                         .count {
                         	margin-left: 2px;
                         	font-size: 8px;
                         }
                         &.active {
                         	color: #fff;
                         }
                         &.positive {
                         	background: rgba(0,160,220,0.2);
                         	&.active {
                         		background: rgba(0,160,220,1);
                         	}
                         }
                         &.negative {
                         	background: rgba(77,85,93,0.2);
                         	&.active {
                         		background: rgba(77,85,93,1);
                         	}
                         }
                   }
	}
	.switch {
                     padding: 12px 18px;
                     line-height: 24px;
                     border-bottom: 1px solid rgba(7,17,27,0.1);
                     color: rgb(147,153,159);
                     &.on {
                     	   .icon-check_circle {
                     	   	color: #00c850;
                     	   }
                     }
                     .icon-check_circle {
                     	           display: inline-block;
                     	           vertical-align: top;
                     	           font-size: 24px;
                     	           margin-right: 4px;
                     }
                     .text {
                     	           font-size: 12px;
                     }
	}
}
</style>

