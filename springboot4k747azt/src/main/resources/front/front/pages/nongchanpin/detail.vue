
<template>
<view>
<mescroll-uni @init="mescrollInit" :up="upOption" :down="downOption" @down="downCallback" @up="upCallback">
	<view class="content">
		<view class="container" :style='{"width":"100%","padding":"20rpx","position":"relative","background":"#DCF7FF","height":"auto"}'>
			<swiper :style='{"width":"100%","background":"#fff","height":"360rpx"}' class="swiper" :indicator-dots='false' :autoplay='false' :circular='false' indicator-active-color='#000000' indicator-color='rgba(0, 0, 0, .3)' :duration='500' :interval='5000' :vertical='false'>
				<swiper-item :style='{"width":"100%","background":"none","height":"360rpx"}' v-for="(swiper,index) in swiperList" :key="index">
					<image :style='{"width":"100%","objectFit":"cover","borderRadius":"20rpx","display":"block","height":"360rpx"}' mode="aspectFill" v-if="swiper.substring(0,4)=='http'" :src="swiper" @tap="imgView(swiper)"></image>
					<image :style='{"width":"100%","objectFit":"cover","borderRadius":"20rpx","display":"block","height":"360rpx"}' mode="aspectFill" v-else :src="baseUrl+swiper" @tap="imgView(baseUrl+swiper)"></image>
				</swiper-item>
			</swiper>
			<view :style='{"width":"100%","padding":"0","background":"none","height":"auto"}' class="detail-content">
				<view :style='{"padding":"10rpx 20rpx","flexWrap":"wrap","background":"#fff","display":"flex","width":"100%","justifyContent":"space-between","height":"auto"}' class="detail-list-item price priceFavor">
					<view :style='{"display":"flex"}' class="text">
						<view :style='{"color":"red","lineHeight":"80rpx","fontSize":"48rpx"}'>￥</view>
						<view :style='{"color":"red","lineHeight":"80rpx","fontSize":"32rpx"}'>{{detail.price}}</view>
					</view>
					<view :style='{"display":"flex"}' v-if="storeupFlag==1" @click="shoucang">
						<text class="icon iconfont icon-shoucang11" :style='{"margin":"0 4rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#666"}'></text>
						<text :style='{"color":"#666","lineHeight":"80rpx","fontSize":"28rpx"}'>已收藏</text>
					</view>
					<view :style='{"display":"flex"}' v-if="storeupFlag==0" @click="shoucang">
						<text class="icon iconfont icon-shoucang11" :style='{"margin":"0 4rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#666"}'></text>
						<text :style='{"color":"#666","lineHeight":"80rpx","fontSize":"28rpx"}'>收藏</text>
					</view>
				</view>
				<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1% 10rpx","color":"rgb(255, 255, 255)","background":"#37E8DC","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' @tap="couponClick">优惠券</button>

				<view :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="detail-list-item title">
					<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}' class="lable">产品名称：</view>
					<view :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' class="text" >{{detail.chanpinmingcheng}}</view>
				</view>
				<view :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="detail-list-item title">
					<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}' class="lable">产品类型：</view>
					<view :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' class="text" >{{detail.chanpinleixing}}</view>
				</view>

				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>简介：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.jianjie}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>产地：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.chandi}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>采摘日期：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.caizhairiqi}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>商家账号：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' style="text-decoration: underline" @tap="merchantClick('shangjiazhanghao')" >{{detail.shangjiazhanghao}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>商家姓名：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.shangjiaxingming}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>库存：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.alllimittimes}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>评论数：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.discussnum}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>评分：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.totalscore}}</view>
				</view>
				<view class="detail-list-item" :style='{"borderColor":"#E8E8E8","background":"#fff","borderWidth":"0 0 2rpx","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
					<view class="lable" :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#333","textAlign":"right"}'>收藏数：</view>
					<view class="text" :style='{"padding":"0px","margin":"0px","lineHeight":"80rpx","fontSize":"28rpx","color":"rgb(0, 0, 0)"}' >{{detail.storeupnum}}</view>
				</view>

				<view :style='{"width":"100%","overflow":"hidden","borderRadius":"0 0 20rpx 20rpx","display":"flex"}'>
					<view :style='{"margin":"0","background":"linear-gradient(48.33deg, rgb(25, 191, 240) 0.51%,rgb(55, 232, 220) 100%)","display":"flex","width":"100%","justifyContent":"center","height":"auto","order":"1"}' class="detail-list-item" v-if="!thumbsupFlag&&!crazilyFlag" @tap="zan">
						<view :style='{"padding":"0","lineHeight":"80rpx","fontSize":"28rpx","color":"#fff","textAlign":"right"}' class="lable">赞</view>
						<view :style='{"padding":"0 20rpx","lineHeight":"80rpx","fontSize":"28rpx","color":"#fff"}'>{{detail.thumbsupnum}}</view>
						<view :style='{"color":"#fff","lineHeight":"80rpx","fontSize":"28rpx"}' class="icon iconfont icon-zan10"></view>
					</view>
					<view :style='{"width":"100%","margin":"0","background":"linear-gradient(48.33deg, rgb(25, 191, 240) 0.51%,rgb(55, 232, 220) 100%)","justifyContent":"center","display":"flex","height":"auto"}' class="detail-list-item" v-if="thumbsupFlag" @tap="zan">
						<view :style='{"padding":"0","lineHeight":"80rpx","fontSize":"28rpx","color":"#fff","textAlign":"right"}' class="lable">取消赞</view>
						<view :style='{"padding":"0 20rpx","lineHeight":"80rpx","fontSize":"28rpx","color":"#fff"}'>{{detail.thumbsupnum}}</view>
						<view :style='{"color":"#fff","lineHeight":"80rpx","fontSize":"28rpx"}' class="icon iconfont icon-zan10"></view>
					</view>
					<view :style='{"width":"100%","margin":"0","background":"#EDEDED","justifyContent":"center","display":"flex","height":"auto"}' class="detail-list-item" v-if="!thumbsupFlag&&!crazilyFlag" @tap="cai">
						<view :style='{"padding":"0","lineHeight":"80rpx","fontSize":"28rpx","color":"#999898","textAlign":"right"}' class="lable">踩</view>
						<view :style='{"padding":"0 20rpx","lineHeight":"80rpx","fontSize":"28rpx","color":"#999898"}'>{{detail.crazilynum}}</view>
						<view :style='{"color":"#999898","lineHeight":"80rpx","fontSize":"28rpx"}' class="icon iconfont icon-cai11"></view>
					</view>
					<view :style='{"width":"100%","margin":"0 0 24rpx 0","background":"#EDEDED","justifyContent":"center","display":"flex","height":"auto"}' class="detail-list-item" v-if="crazilyFlag" @tap="cai">
						<view :style='{"padding":"0","lineHeight":"80rpx","fontSize":"28rpx","color":"#999898","textAlign":"right"}' class="lable">取消踩</view>
						<view :style='{"padding":"0 20rpx","lineHeight":"80rpx","fontSize":"28rpx","color":"#999898"}'>{{detail.crazilynum}}</view>
						<view :style='{"color":"#999898","lineHeight":"80rpx","fontSize":"28rpx"}' class="icon iconfont icon-cai11"></view>
					</view>
				</view>






				<view class="time-content" :style='{"width":"100%","margin":"0 0 24rpx 0","height":"auto"}'>
					<view class="comoment-header" :style='{"border":"2rpx solid #37E8DC","overflow":"hidden","borderRadius":"10rpx","background":"#fff","display":"flex","width":"100%","justifyContent":"space-between","height":"auto"}'>
						<view :style='{"padding":"0 24rpx","lineHeight":"80rpx","fontSize":"30rpx","color":"#37E8DC","fontWeight":"bold"}'>评论</view>
						<view :style='{"padding":"0 20rpx","background":"#37E8DC","display":"flex"}' @click="onCommentTap" class="btn-comment-content" style="display: flex;align-items: center;">
							<view :style='{"margin":"0 8rpx 0 0","lineHeight":"80rpx","fontSize":"28rpx","color":"#fff"}' class="cuIcon-add"></view>
							<view :style='{"color":"#fff","lineHeight":"80rpx","fontSize":"28rpx"}'>添加评论</view>
						</view>
					</view>
				
					<view :style='{"boxShadow":"0 0px 0px rgb(0 0 0 / 30%)","padding":"20rpx","margin":"24rpx 0 0 0","borderRadius":"20rpx","flexWrap":"wrap","background":"#fff","display":"flex","width":"100%","position":"relative","height":"auto"}' v-for="(item,index) in commentList" v-bind:key="index" class="cu-item comment-item">
						<view :style='{"boxShadow":"0 0px 0px rgba(0,0,0,.1)","position":"absolute","top":"0","left":"0","background":"none"}' v-if="item.istop">
							<span class="icon iconfont icon-jiantou24" :style='{"color":"#f00","lineHeight":"1"}'></span>
						</view>
						<view :style='{"width":"100%","display":"flex","height":"auto","order":"1"}'>
							<image :style='{"width":"60rpx","margin":"0 10rpx 0 0","borderRadius":"100%","display":"block","height":"60rpx"}' v-if="item.avatarurl" mode="aspectFill" :src="baseUrl+item.avatarurl"></image>
							<view :style='{"color":"#333","lineHeight":"60rpx","fontSize":"28rpx"}' class="text-grey">{{item.nickname}}</view>
						</view>
						<view :style='{"width":"100%","margin":"8rpx 0 8rpx 70rpx","lineHeight":"1.5","fontSize":"28rpx","color":"#666","order":"3"}' class="text-gray text-content text-df">
							<rich-text :nodes="item.content"></rich-text>
						</view>
						<view :style='{"width":"100%","margin":"8rpx 0 8rpx 70rpx","lineHeight":"1.5","fontSize":"28rpx","color":"#666","order":"3"}' class="text-gray text-content text-df">
							<uni-rate disabled v-model="item.score" disabled-color="#ffca3e" />
						</view>
						<view :style='{"padding":"0 0 0 70rpx","margin":"0","alignItems":"center","display":"flex","width":"100%","justifyContent":"space-between","height":"60rpx","order":"2"}'>
							<view v-if="!comzanChange(item)&&!comcaiChange(item)" :style='{"border":"2rpx solid #37E8DC","padding":"0 10rpx","alignItems":"center","borderRadius":"8rpx","display":"flex","order":"1"}' @click="comzanClick(item)">
								<span class="icon iconfont icon-zan07" :style='{"color":"#37E8DC","fontSize":"26rpx","lineHeight":"40rpx"}'></span>
								<span :style='{"fontSize":"28rpx","display":"none"}'>赞</span>
								<span :style='{"color":"#37E8DC","fontSize":"26rpx","lineHeight":"40rpx"}'>({{item.thumbsupnum}})</span>
							</view>
							<view v-if="comzanChange(item)" :style='{"padding":"0 10rpx","alignItems":"center","borderRadius":"8rpx","background":"#37E8DC","display":"flex","order":"1"}' @click="comzanClick(item)">
								<span class="icon iconfont icon-zan11" :style='{"color":"#fff","fontSize":"26rpx","lineHeight":"40rpx"}'></span>
								<span :style='{"color":"#ff0000","fontSize":"28rpx","display":"none"}'>已赞</span>
								<span :style='{"color":"#fff","fontSize":"26rpx","lineHeight":"40rpx"}'>({{item.thumbsupnum}})</span>
							</view>
							<view v-if="!comzanChange(item)&&!comcaiChange(item)" :style='{"border":"2rpx solid #CCCCCC","padding":"0 10rpx","alignItems":"center","borderRadius":"8rpx","display":"flex"}' @click="comcaiClick(item)">
								<span class="icon iconfont icon-cai01" :style='{"color":"#CCCCCC","fontSize":"26rpx","lineHeight":"40rpx"}'></span>
								<span :style='{"fontSize":"28rpx","display":"none"}'>踩</span>
								<span :style='{"color":"#CCCCCC","fontSize":"26rpx","lineHeight":"40rpx"}'>({{item.crazilynum}})</span>
							</view>
							<view v-if="comcaiChange(item)" :style='{"padding":"0 10rpx","alignItems":"center","borderRadius":"8rpx","background":"#CCCCCC","display":"flex"}' @click="comcaiClick(item)">
								<span class="icon iconfont icon-cai16" :style='{"color":"#fff","fontSize":"26rpx","lineHeight":"40rpx"}'></span>
								<span :style='{"color":"#ff0000","fontSize":"28rpx","display":"none"}'>已踩</span>
								<span :style='{"color":"#fff","fontSize":"26rpx","lineHeight":"40rpx"}'>({{item.crazilynum}})</span>
							</view>
						</view>
						<view :style='{"lineHeight":"60rpx","fontSize":"24rpx","position":"absoluter","right":"20rpx","color":"#999"}' class="margin-top-sm text-gray text-df">{{item.addtime}}</view>
						<view v-if="item.reply" :style='{"width":"100%","margin":"8rpx 0 8rpx 70rpx","lineHeight":"1.5","fontSize":"28rpx","color":"#666","order":"3"}' class="text-gray text-content text-df">
							回复:<rich-text :nodes="item.reply"></rich-text>
						</view>
						<view style="display: flex;display: flex;justify-content: flex-end;padding: 6rpx 0;" v-if="user&&user.id==item.userid">
							<view style="color: #999;font-size: 16rpx;" @click="delClick(item.id)">删除</view>
						</view>
					</view>
				</view>

				<view class="bottom-content bg-white tabbar border shop" :style='{"width":"100%","padding":"0","flexWrap":"wrap","background":"none","display":"flex","height":"auto"}'>

					<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1% 10rpx","color":"rgb(255, 255, 255)","background":"#4EA0B8","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' v-if="userid&&isAuth('nongchanpin','纠纷投诉')" @tap="onAcrossTap('jiufentousu','','','','')" class="cu-btn bg-brown round shadow-blur" >纠纷投诉</button>
					<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1% 10rpx","color":"rgb(255, 255, 255)","background":"#4EA0B8","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' v-if="!userid&&isAuthFront('nongchanpin','纠纷投诉')" @tap="onAcrossTap('jiufentousu','','','','')" class="cu-btn bg-brown round shadow-blur" >纠纷投诉</button>
					<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1%","color":"rgb(255, 255, 255)","background":"#4EA0B8","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' v-if="detail.alllimittimes>0" @tap="onCartTap" class="cu-btn bg-orange round shadow-blur" >加入购物车</button>
					<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1% 10rpx","color":"rgb(255, 255, 255)","background":"#37E8DC","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' v-if="detail.alllimittimes>0" @tap="onBuyTap">立即购买</button>
					<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1%","color":"rgb(255, 255, 255)","background":"#4EA0B8","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' v-if="userid&&isAuth('nongchanpin','私聊')" @tap="chatClick">联系TA</button>
					<button :style='{"border":"0","padding":"0 20rpx","margin":"0 1%","color":"rgb(255, 255, 255)","background":"#4EA0B8","width":"calc(100% / 3 - 2%)","fontSize":"28rpx","lineHeight":"80rpx","height":"80rpx"}' v-if="!userid&&isAuthFront('nongchanpin','私聊')" @tap="chatClick">联系TA</button>
					
				</view>
			</view>
		</view>
		<uni-popup class="popup-content" ref="couponPopup" type="bottom">
			<view class="couponList">
				<view class="coupon" v-for="(item,index) in couponList" :key="index">
					<view class="coupon_left">
						<view class="name">{{item.name}}</view>
						<view class="name">满{{item.fullamount}}减{{item.discountamount}}</view>
						<view class="date">{{item.startime}}-{{item.endtime}}</view>
						<view class="date">{{item.remark}}</view>
						<view class="date1" v-if="item.shangjiazhanghao">仅限 {{item.shangjiazhanghao}} 商品可使用</view>
						<view class="date1" v-else>全品类商品可使用</view>
					</view>
					<view class="coupon_right">
						<view class="price">￥<span class="num">{{item.discountamount}}</span></view>
						<view class="btn" @click="getCoupon(item)">立即领取</view>
					</view>
				</view>
			</view>
		</uni-popup>
	</view>
</mescroll-uni>
</view>
</template>

<script>
	import uniRate from "@/components/uni-rate/uni-rate.vue";
	export default {
		data() {
			return {
				btnColor: ['#409eff','#67c23a','#909399','#e6a23c','#f56c6c','#356c6c','#351c6c','#f093a9','#a7c23a','#104eff','#10441f','#a21233','#503319'],
				id: '',
				userid: '',
				detail: {},
				swiperList: [],
				commentList: [],
				mescroll: null, //mescroll实例对象
				downOption: {
					auto: false //是否在初始化后,自动执行下拉回调callback; 默认true
				},
				upOption: {
					noMoreSize: 3, //如果列表已无数据,可设置列表的总数量要大于半页才显示无更多数据;避免列表数据过少(比如只有一条数据),显示无更多数据会不好看; 默认5
					textNoMore: '~ 没有更多了 ~',
				},
				hasNext: true,
				user: {},
				storeupFlag: 0,
				thumbsupFlag: 0,
				crazilyFlag: 0,
				count: 0,
				timer: null,
				title:'',
				couponList: [],
			}
		},
		components: {
			uniRate,
		},
		computed: {
			baseUrl() {
				return this.$base.url;
			},
		},
		async onLoad(options) {
			// #ifdef APP-PLUS
			let page = getCurrentPages()
			this.href = this.baseUrl + 'front/h5/#/' + page[page.length - 1].route
			// #endif
			let table = uni.getStorageSync("nowTable");
			// 获取用户信息
			let res = await this.$api.session(table);
			this.user = res.data;
			this.id = options.id;
			if(options.userid) {
				this.userid = options.userid;
			}
			// 渲染数据
			this.init();
			this.hasNext = true
			// 重新加载数据
			if (this.mescroll) this.mescroll.resetUpScroll()
		},
		// #ifdef MP-WEIXIN
		onShareAppMessage(){
			var obj = {
				title: this.title,
				imageUrl: this.swiperList[0]?this.baseUrl + this.swiperList[0]: ''
			}
			return obj
		},
		// #endif
		onUnload() {
			if(this.timer) {
				clearInterval(this.timer);
			}
		},
		async onShow(options) {
			let table = uni.getStorageSync("nowTable");
			// 获取用户信息
			let res = await this.$api.session(table);
			this.user = res.data;
			this.btnColor = this.btnColor.sort(()=> {
				return (0.5-Math.random());
			});
			this.getStoreup();
			this.getThumbsup();
			let cleanType = uni.getStorageSync('discussnongchanpinCleanType')
			if(cleanType){
				uni.removeStorageSync('discussnongchanpinCleanType')
				this.mescroll.num = 1
				this.upCallback(this.mescroll)
				this.init(2);
			}
			let crossCleanType = uni.getStorageSync('crossCleanType')
			if(crossCleanType) {
				uni.removeStorageSync('crossCleanType')
				res = await this.$api.info('nongchanpin', this.id);
				let reg=new RegExp('http://localhost:8080/springboot4k747azt/upload','g')//g代表全部
				this.detail = res.data;
				this.title = this.detail.chanpinmingcheng
			}
		},
		destroyed: function() {
			//window.clearInterval(this.inter);
		},
		methods: {
			imgView(url){
				let arr = []
				for(let x in this.swiperList){
					arr.push(this.swiperList[x].substr(0,4)=='http'?this.swiperList[x]:this.baseUrl + this.swiperList[x])
				}
				uni.previewImage({
					current: url,
					urls: arr
				})
			},
			// 拨打电话
			callClick(row){
				uni.makePhoneCall({
					phoneNumber: row
				})
			},
			async couponClick(){
				this.couponList = []
				let res = await this.$api.lists('coupon')
				if(res&&res.code==0) {
					for(let x in res.data){
						if (this.changeCoupon(res.data[x])) {
							this.couponList.push(res.data[x])
						}
					}
				}
				if(this.couponList.length){
					this.$refs.couponPopup.open()
				}else {
					this.$utils.msg('暂无可领取优惠券')
				}
			},
			changeCoupon(row) {
				if (!this.isBetweenTime(this.$utils.getCurDateTime(), [row.startime, row.endtime])) {
					return false
				}
				if(row.shangjiazhanghao&&this.detail.shangjiazhanghao != row.shangjiazhanghao){
					return false
				}
				return true
			},
			isBetweenTime(currentDate, timeQuantum) {
				let isBetween = true;
				let currentTime = new Date(currentDate);
				let startTime = new Date(timeQuantum[0]);
				let endTime = new Date(timeQuantum[1]);
				let t1 = currentTime.getTime() - startTime.getTime();
				let t2 = currentTime.getTime() - endTime.getTime();
				if (t1 < 0 || t2 > 0) {
					isBetween = false;
				}
				return isBetween;
			},
			async getCoupon(item){
				let res = await this.$api.page('mycoupon',{page:1,couponid: item.id})
				if(res.data.list.length){
					this.$utils.msg('已领取该优惠券，请前往使用')
					return false
				}
				let data = {
					couponid: item.id,
				}
				data = Object.assign(data, JSON.parse(JSON.stringify(item)))
				data.couponnumber = this.getUUID()
				data.userid = uni.getStorageSync('appUserid')
				delete data.id
				delete data.addtime
				await this.$api.add('mycoupon',data)
				this.$utils.msg('领取成功！')
			},
			getUUID() {
				return new Date().getTime();
			},
			// 支付
			onPayTap(){
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				uni.setStorageSync('paytable','nongchanpin');
				uni.setStorageSync('payObject',this.detail);
				this.$utils.jump('../pay-confirm/pay-confirm?type=1')
			},
			async merchantClick(name){
				let res = await this.$api.query('shangjia',{shangjiazhanghao: this.detail.shangjiazhanghao})
				this.$utils.jump(`../shangjia/detail?id=${res.data.id}`)
			},
			onDetailTap(item) {
				uni.setStorageSync("useridTag",this.userid);
				this.$utils.jump(`./detail?id=${item.id}&userid=`+this.userid)
			},
			// 收藏
			async getStoreup() {
				if(!this.user){
					return false
				}
				let params = {
					page: 1,
					limit: 1,
					refid : this.id,
					tablename : 'nongchanpin',
					userid: this.user.id,
					type: 1,
				}
				let res = await this.$api.list(`storeup`, params);
				this.storeupFlag = res.data.list.length;
			},
			async shoucang(){
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				let _this = this;
				let params = {
					page: 1,
					limit: 1,
					refid : _this.detail.id,
					tablename : 'nongchanpin',
					userid: _this.user.id,
					type: 1,
				}
				let res = await _this.$api.list(`storeup`, params);
				if (res.data.list.length == 1) {
					let storeupId = res.data.list[0].id;
					uni.showModal({
						title: '提示',
						content: '是否取消',
						success: async function(res) {
							if (res.confirm) {
								_this.detail.storeupnum--
								await _this.$api.update('nongchanpin',_this.detail)
								await _this.$api.del('storeup', JSON.stringify([storeupId]));
								_this.$utils.msg('取消成功');
								_this.getStoreup();
							}
						}
					});
					return;
				}
				uni.showModal({
					title: '提示',
					content: '是否收藏',
					success: async function(res) {
						if (res.confirm) {
							await _this.$api.add('storeup', {
								userid: _this.user.id,
								name: _this.detail.chanpinmingcheng,
								inteltype: _this.detail.chanpinleixing,
								picture: _this.swiperList[0],
								refid: _this.detail.id,
								tablename: 'nongchanpin',
								type: 1
							});
							_this.detail.storeupnum++
							await _this.$api.update('nongchanpin',_this.detail)
							_this.$utils.msg('收藏成功');
							_this.getStoreup();
						}
					}
				});
			},
			// 跨表
			async onAcrossTap(tableName,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue,type=1){
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				uni.setStorageSync('crossTable','nongchanpin');
				uni.setStorageSync(`crossObj`, this.detail);
				uni.setStorageSync(`statusColumnName`, statusColumnName);
				uni.setStorageSync(`statusColumnValue`, statusColumnValue);
				uni.setStorageSync(`tips`, tips);
				if(statusColumnName!=''&&!statusColumnName.startsWith("[")) {
					var obj = uni.getStorageSync('crossObj');
					for (var o in obj){
						if(o==statusColumnName && obj[o]==statusColumnValue){
							this.$utils.msg(tips);
							return
						}
					}
				}
				this.$utils.jump(`../${tableName}/add-or-update?cross=true`);
			},
			// 获取详情
			async init(type=1){
				if(this.timer) {
					clearInterval(this.timer);
				}
				let res = await this.$api.info('nongchanpin', this.id);
				let reg=new RegExp('http://localhost:8080/springboot4k747azt/upload','g')//g代表全部
				this.detail = res.data;

				this.title = this.detail.chanpinmingcheng


				// 轮播图片
				this.swiperList = this.detail.tupian ? this.detail.tupian.split(",") : [];
				
















				if(type==2){
					this.detail.discussnum++
					await this.$api.update('nongchanpin',this.detail)
				}
			},
			// mescroll组件初始化的回调,可获取到mescroll对象
			mescrollInit(mescroll) {
				this.mescroll = mescroll;
			},

			/*下拉刷新的回调 */
			downCallback(mescroll) {
				this.hasNext = true
				mescroll.resetUpScroll()
			},

			/*上拉加载的回调: mescroll携带page的参数, 其中num:当前页 从1开始, size:每页数据条数,默认10 */
			async upCallback(mescroll) {
				if(uni.getStorageSync("appUserid")){
					let res = await this.$api.list('discussnongchanpin', {
						page: mescroll.num,
						limit: 10,
						refid: Number(this.id)
					});
					// 如果是第一页数据置空
					if (mescroll.num == 1) this.commentList = [];
					for(let x in res.data.list){
						if(res.data.list[x].content){
							res.data.list[x].content = res.data.list[x].content.replace(/img src/gi,"img style=\"width:100%;\" src")
						}
					}
					this.commentList = this.commentList.concat(res.data.list);
					if (res.data.list.length == 0) this.hasNext = false;
				}
				mescroll.endSuccess(mescroll.size, this.hasNext);

			},
			comzanChange(row){
				if(row.tuserids){
					let arr = String(row.tuserids).split(',')
					for(let x in arr){
						if(arr[x] == this.user.id){
							return true
						}
					}
				}
				return false
			},
			async comzanClick(row){
				if(!this.user){
					return false
				}
				if(!this.comzanChange(row)){
					row.thumbsupnum++
					if(row.tuserids){
						row.tuserids = row.tuserids + ',' + this.user.id
					}else {
						row.tuserids = this.user.id
					}
					await this.$api.update('discussnongchanpin',row)
					this.$utils.msg('点赞成功');
				}else {
					row.thumbsupnum--
					let arr = String(row.tuserids).split(',')
					for(let x in arr){
						if(arr[x] == this.user.id){
							arr.splice(x,1)
						}
					}
					row.tuserids = arr.join(',')
					await this.$api.update('discussnongchanpin',row)
					this.$utils.msg('取消成功');
				}
				this.$forceUpdate()
			},
			comcaiChange(row){
				if(row.cuserids){
					let arr = String(row.cuserids).split(',')
					for(let x in arr){
						if(arr[x] == this.user.id){
							return true
						}
					}
				}
				return false
			},
			async comcaiClick(row){
				if(!this.user){
					return false
				}
				if(!this.comcaiChange(row)){
					row.crazilynum++
					if(row.cuserids){
						row.cuserids = row.cuserids + ',' + this.user.id
					}else {
						row.cuserids = this.user.id
					}
					await this.$api.update('discussnongchanpin',row)
					this.$utils.msg('点踩成功');
				}else {
					row.crazilynum--
					let arr = String(row.cuserids).split(',')
					for(let x in arr){
						if(arr[x] == this.user.id){
							arr.splice(x,1)
						}
					}
					row.cuserids = arr.join(',')
					await this.$api.update('discussnongchanpin',row)
					this.$utils.msg('取消成功');
				}
				this.$forceUpdate()
			},

			async onCartTap() {
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				if (uni.getStorageSync(`cart${this.detail.id}${this.user.id}`)) {
					this.$utils.msg('该商品已添加到购物车')
					return
				}
				await this.$api.add('cart', {
					tablename: 'nongchanpin',
					goodid: this.detail.id,
					goodname: this.detail.chanpinmingcheng,
					shangjiazhanghao: this.detail.shangjiazhanghao,
					goodtype: this.detail.chanpinleixing,
					picture: this.swiperList[0],
					buynumber: 1,
					userid: this.user.id,
					price: this.detail.price,
					discountprice: this.detail.vipprice
				});
				uni.setStorageSync(`cart${this.detail.id}${this.user.id}`, true);
				this.$utils.msg('添加到购物车成功')
			},
			onBuyTap() {
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				uni.setStorageSync('orderGoods', [{
					tablename: 'nongchanpin',
					goodid: this.detail.id,
					goodname: this.detail.chanpinmingcheng,
					shangjiazhanghao: this.detail.shangjiazhanghao,
					goodtype: this.detail.chanpinleixing,
					picture: this.swiperList[0],
					buynumber: 1,
					price: this.detail.price,
					discountprice: this.detail.vipprice
				}])
				this.$utils.jump('../shop-order-confirm/shop-order-confirm?type=1');
			},


			onChatTap() {
				this.$utils.jump('../chat/chat')
			},
			// 下载
			download(url ){
				if(!url){
					return false
				}
				let _this = this;
				url=_this.$base.url +  url;
				uni.downloadFile({
					url: url,
					success: (res) => {
						if (res.statusCode === 200) {
							_this.$utils.msg('下载成功');
							// #ifdef H5
							 window.open(url);
							// #endif
							// #ifndef H5
							uni.saveFile({
								tempFilePath: res.tempFilePath, //临时路径
								success: function(obj) {
									uni.showToast({
										icon: 'success',
										mask: true,
										title: '下载成功' , 
										duration: 2000,
									});
									setTimeout(() => {
										console.log('obj.savedFilePath',obj.savedFilePath);
										var filePath = obj.savedFilePath;
										uni.openDocument({ //新开页面打开文档，支持格式：doc, xls, ppt, pdf, docx, xlsx, pptx。
											filePath: filePath,
											showMenu: true,
											success: function(res) {
												console.log('打开文档成功');
											}
										});
									}, 2000)
								}
							});
							// #endif
						}
					}
				});
			},
			//
			onCartTabTap() {
				this.$utils.tab('../shop-cart/shop-cart')
			},
			// 添加评论
			async onCommentTap() {
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				let res = {}
				let params = {
					page: 1,
					limit: 1,
					status: '已完成',
					goodid: this.detail.id,
					userid: uni.getStorageSync('appUserid'),
				}
				res = await this.$api.list('orders', params);
				if (res.data.list.length == 0) {
				this.$utils.msg('请完成订单后再评论');
					return;
				}
				let obj = {
					page: 1,
					limit: 1,
					refid: this.id,
					userid: uni.getStorageSync('appUserid')
				}
				res = await this.$api.page('discussnongchanpin',obj)
				if(res.data.list.length){
					this.$utils.msg('每个人只能评论一次');
					return;
				}
				this.$utils.jump(`../discussnongchanpin/add-or-update?refid=${this.id}`  + '&virtualPay=1' )
			},
			delClick(id){
				let that = this
				uni.showModal({
					title: '提示',
					content: '是否删除此评论？',
					async success(res) {
						if(res.confirm){
							await that.$api.del('discussnongchanpin',JSON.stringify([id]))
							that.$utils.msg('删除成功')
							that.detail.discussnum--
							await that.$api.update('nongchanpin',that.detail)
							setTimeout(()=>{
								that.upCallback(that.mescroll)
							},1500)
						}
					}
				})
			},
			// 获取赞踩
			async getThumbsup() {
				if(!this.user){
					return false
				}
				let params = {
					page: 1,
					limit: 1,
					refid: this.id,
					tablename: 'nongchanpin',
					userid: this.user.id,
					type: '%2%',
				}
				let res = await this.$api.list(`storeup`, params);
				if (res.data.list.length > 0) {
					if(res.data.list[0].type=='21') {
						this.thumbsupFlag = 1;
					} else {
						this.crazilyFlag = 1;
					}
				}

			},
			// 点赞
			async zan() {
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				let _this = this;
				let params = {
					page: 1,
					limit: 1,
					refid: _this.detail.id,
					tablename : 'nongchanpin',
					userid: _this.user.id,
					type: '%2%',
				}
				let res = await _this.$api.list(`storeup`, params);
				if (res.data.list.length > 0) {
					let storeupId = res.data.list[0].id;
					uni.showModal({
						title: '提示',
						content: '是否取消点赞',
						success: async function(res) {
							if (res.confirm) {
								await _this.$api.del('storeup', JSON.stringify([storeupId]));
								_this.detail.thumbsupnum = parseInt(_this.detail.thumbsupnum) - 1;
								await _this.$api.update('nongchanpin', _this.detail);
								_this.$utils.msg('取消成功');
								_this.thumbsupFlag=0;
							}
						}
					});
					return;
				}
				uni.showModal({
					title: '提示',
					content: '是否点赞',
					success: async function(res) {
						if (res.confirm) {
							await _this.$api.add('storeup', {
								userid: _this.user.id,
								name: _this.detail.chanpinmingcheng,
								picture: _this.swiperList[0],
								refid: _this.detail.id,
								tablename: 'nongchanpin',
								type: '21'
							});
							_this.detail.thumbsupnum = parseInt(_this.detail.thumbsupnum) + 1;
							await _this.$api.update('nongchanpin', _this.detail);
							_this.$utils.msg('点赞成功');
							_this.thumbsupFlag=1;
						}
					}
				});
			},
			// 踩
			async cai() {
				let that = this
				if(!this.user){
					this.$utils.msg("请先登录")
					setTimeout(()=>{
						that.$utils.jump('../login/login')
					},1500)
					return false
				}
				let _this = this;
				let params = {
					page: 1,
					limit: 1,
					refid: _this.detail.id,
					tablename: 'nongchanpin',
					userid: _this.user.id,
					type: '%2%',
				}
				let res = await _this.$api.list(`storeup`, params);
				if (res.data.list.length > 0) {
					let storeupId = res.data.list[0].id;
					uni.showModal({
						title: '提示',
						content: '是否取消点踩',
						success: async function(res) {
							if (res.confirm) {
								await _this.$api.del('storeup', JSON.stringify([storeupId]));
								_this.detail.crazilynum = parseInt(_this.detail.crazilynum) - 1;
								await _this.$api.update('nongchanpin', _this.detail);
								_this.$utils.msg('取消成功');
								_this.crazilyFlag=0;
							}
						}
					});
					return;
				}
				uni.showModal({
					title: '提示',
					content: '是否点踩',
					success: async function(res) {
						if (res.confirm) {
							await _this.$api.add('storeup', {
								userid: _this.user.id,
								name: _this.detail.chanpinmingcheng,
								picture: _this.swiperList[0],
								refid: _this.detail.id,
								tablename: 'nongchanpin',
								type: '22'
							});
							_this.detail.crazilynum = parseInt(_this.detail.crazilynum) + 1;
							await _this.$api.update('nongchanpin', _this.detail);
							_this.$utils.msg('点踩成功');
							_this.crazilyFlag=1;
						}
					}
				});
			},
		}
	}
</script>

<style lang="scss">
	page {
	  --animate-duration: 1s;
	  --animate-delay: 1s;
	  --animate-repeat: 1;
	}
	
	.content {
		min-height: calc(100vh - 44px);
		box-sizing: border-box;
	}
	
	.seat-list {
		display: flex;
		align-items: center;
		flex-wrap: wrap;
		background: #FFFFFF;
		margin: 20upx;
		border-radius: 20upx;
		padding: 20upx;
		font-size: 30upx;
		.seat-item {
			width: 33.33%;
			display: flex;
			align-items: center;
			flex-direction: column;
			margin-bottom: 20upx;
	
			.seat-icon {
				width: 50upx;
				height: 50upx;
				margin-bottom: 10upx;
			}
		}
	}
	
	audio {
		display: flex;
		flex-direction: column;
	}
	
	.audio /deep/ .uni-audio-default {
		width: inherit;
	}
	

	.couponList {
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		background: #fff;
		padding: 20px 20px;
		max-height: 60vh;
		overflow-y: scroll;
		.coupon {
			width: 100%;
			background: url('../../static/coupon/coupon.png');
			background-repeat: no-repeat;
			background-position: center center;
			background-size: cover;
			padding: 60px 30px;
			display: flex;
			align-items: center;
			.coupon_left {
				flex: 1;
				color: #000;
				
				.name {
					font-size: 22px;
					line-height: 1.5;
				}
				
				.date {
					font-size: 14px;
					line-height: 1.5;
				}
				.date1 {
					font-size: 12px;
					line-height: 1.5;
				}
			}
				
			.coupon_right {
				display: flex;
				flex-direction: column;
				align-items: center;
				
				.price {
					color: #FF0800;
					font-size: 20px;
					padding: 0 0 10px;
				
					.num {
						font-size: 28px;
					}
				}
				
				.btn {
					width: 100px;
					height: 30px;
					line-height: 30px;
					border-radius: 30px;
					background: #FF0800;
					text-align: center;
					color: #fff;
					font-size: 18px;
					cursor: pointer;
				}
			}
		}
	}
</style>
