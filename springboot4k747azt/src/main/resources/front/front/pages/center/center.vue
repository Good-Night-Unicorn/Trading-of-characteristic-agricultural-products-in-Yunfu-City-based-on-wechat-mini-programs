<template>
	<view class="content">
		<view :style='{"width":"100%","padding":"20rpx","position":"relative","background":"#DCF7FF","height":"auto"}'>
			<view v-if="user&&user.id" :style='{"padding":"0","margin":"0 0 20rpx 0","background":"none","display":"flex","width":"100%","position":"relative","height":"auto"}' @tap="onPageTap('../user-info/user-info')" class="header" v-bind:class="{'status':isH5Plus}">
				<view :style='{"alignContent":"center","alignItems":"center","flexWrap":"wrap","flex":"1","display":"flex","height":"100%"}' v-if="tableName=='yonghu'" class="userinfo">
					<image :style='{"width":"100rpx","padding":"0","margin":"0 20rpx 0 0","borderRadius":"100%","height":"100rpx"}' :src="user.touxiang?baseUrl+user.touxiang:require('../../static/gen/upload.png')"></image>
					<view :style='{"width":"calc(100% - 120rpx)","flexDirection":"column","justifyContent":"space-between","display":"flex","height":"100rpx"}' class="info">
						<view :style='{"width":"100%","lineHeight":"36rpx","fontSize":"24rpx","color":"#000"}'>{{user.yonghuzhanghao}}<text v-if="user.vip&& user.vip=='是'">(VIP)</text></view>
					</view>
					
					<view :style='{"width":"100%","padding":"40rpx 0","justifyContent":"space-between","display":"flex"}' class="info">
						<view :style='{"border":"2rpx solid #37E8DC","padding":"16rpx 0","alignItems":"center","borderRadius":"10rpx","flexDirection":"column","background":"#BAEEF1","display":"flex","width":"30%","lineHeight":"36rpx","fontSize":"24rpx"}'>
							<span :style='{"color":"#000"}'>联系方式：</span>
							<span :style='{"color":"#000"}'>{{user.lianxifangshi}}</span>
						</view>
						<view :style='{"border":"2rpx solid #37E8DC","padding":"16rpx 0","alignItems":"center","borderRadius":"10rpx","flexDirection":"column","background":"#BAEEF1","display":"flex","width":"30%","lineHeight":"36rpx","fontSize":"24rpx"}' v-if="user.money">
							<span :style='{"color":"#000"}'>余额：</span>
							<span :style='{"color":"#000"}'>{{user.money}}</span>
						</view>
						<view :style='{"border":"2rpx solid #37E8DC","padding":"16rpx 0","alignItems":"center","borderRadius":"10rpx","flexDirection":"column","background":"#BAEEF1","display":"flex","width":"30%","lineHeight":"36rpx","fontSize":"24rpx"}' v-if="user.jf||user.jf==0">
							<span :style='{"color":"#000"}'>积分：</span>
							<span :style='{"color":"#000"}'>{{user.jf}}</span>
						</view>
					</view>
				</view>
				<view :style='{"alignContent":"center","alignItems":"center","flexWrap":"wrap","flex":"1","display":"flex","height":"100%"}' v-if="tableName=='shangjia'" class="userinfo">
					<image :style='{"width":"100rpx","padding":"0","margin":"0 20rpx 0 0","borderRadius":"100%","height":"100rpx"}' :src="user.touxiang?baseUrl+user.touxiang:require('../../static/gen/upload.png')"></image>
					<view :style='{"width":"calc(100% - 120rpx)","flexDirection":"column","justifyContent":"space-between","display":"flex","height":"100rpx"}' class="info">
						<view :style='{"width":"100%","lineHeight":"36rpx","fontSize":"24rpx","color":"#000"}'>{{user.shangjiazhanghao}}<text v-if="user.vip&& user.vip=='是'">(VIP)</text></view>
					</view>
					
					<view :style='{"width":"100%","padding":"40rpx 0","justifyContent":"space-between","display":"flex"}' class="info">
						<view :style='{"border":"2rpx solid #37E8DC","padding":"16rpx 0","alignItems":"center","borderRadius":"10rpx","flexDirection":"column","background":"#BAEEF1","display":"flex","width":"30%","lineHeight":"36rpx","fontSize":"24rpx"}'>
							<span :style='{"color":"#000"}'>联系方式：</span>
							<span :style='{"color":"#000"}'>{{user.lianxifangshi}}</span>
						</view>
						<view :style='{"border":"2rpx solid #37E8DC","padding":"16rpx 0","alignItems":"center","borderRadius":"10rpx","flexDirection":"column","background":"#BAEEF1","display":"flex","width":"30%","lineHeight":"36rpx","fontSize":"24rpx"}' v-if="user.money">
							<span :style='{"color":"#000"}'>余额：</span>
							<span :style='{"color":"#000"}'>{{user.money}}</span>
						</view>
						<view :style='{"border":"2rpx solid #37E8DC","padding":"16rpx 0","alignItems":"center","borderRadius":"10rpx","flexDirection":"column","background":"#BAEEF1","display":"flex","width":"30%","lineHeight":"36rpx","fontSize":"24rpx"}' v-if="user.jf||user.jf==0">
							<span :style='{"color":"#000"}'>积分：</span>
							<span :style='{"color":"#000"}'>{{user.jf}}</span>
						</view>
					</view>
				</view>
				<view :style='{"padding":"6rpx 20rpx","alignItems":"center","borderRadius":"20rpx","top":"0","background":"linear-gradient(-40.70deg, rgb(28, 195, 238) 5.17%,rgb(55, 232, 220) 93.352%)","display":"flex","width":"auto","position":"absolute","right":"0","justifyContent":"center","height":"auto"}' class="setting">
					<text class="icon iconfont icon-qita6" :style='{"border":"0","width":"auto","lineHeight":"1","fontSize":"28rpx","color":"#fff","borderRadius":"0"}'></text>
					<text :style='{"color":"#fff","lineHeight":"1","fontSize":"24rpx"}'>设置</text>
				</view>
			</view>
			
			<view v-else :style='{"padding":"0","margin":"0 0 20rpx 0","background":"none","display":"flex","width":"100%","position":"relative","height":"auto"}' @tap="loginClick" class="header" v-bind:class="{'status':isH5Plus}">
				<view :style='{"alignItems":"center","color":"#666","display":"flex","width":"100%","fontSize":"36rpx","justifyContent":"center","height":"100%"}'>登录/注册</view>
			</view>
			
			<view :style='{"width":"100%","background":"none","height":"auto"}' class="list">
				<view :style='{"width":"100%","margin":"0 0 20rpx","height":"auto"}' v-if="user&&user.id">
					<view :style='{"width":"100%","padding":"0 20rpx","background":"black","justifyContent":"space-between","display":"none","height":"auto"}'>
						<view :style='{"color":"#fff","fontSize":"28rpx","lineHeight":"72rpx"}'>我的订单</view>
						<view :style='{"color":"#fff","fontSize":"28rpx","lineHeight":"72rpx"}' @tap="onPageTap('../shop-order/shop-order')">查看全部</view>
					</view>
					<view :style='{"border":"2rpx solid rgb(55, 232, 220)","padding":"20rpx 0 12rpx","borderRadius":"20rpx","background":"linear-gradient(134.13deg, rgba(55, 232, 220, 0.5) 0.183%,rgba(28, 195, 238, 0.5) 100%)","display":"flex","width":"100%","height":"auto"}'>
						<view class="list-item" :style='{"alignItems":"center","flex":"1","flexDirection":"column","display":"flex","height":"auto"}' @tap="onPageTap('../shop-order/shop-order?nav=未支付')">
							<span class="icon iconfont icon-dianpu" :style='{"color":"#fff","lineHeight":"1","fontSize":"48rpx"}'></span>
							<view :style='{"color":"#fff","lineHeight":"2","fontSize":"24rpx"}' class="title">未支付</view>
						</view>
						<view class="list-item" :style='{"alignItems":"center","flex":"1","flexDirection":"column","display":"flex","height":"auto"}' @tap="onPageTap('../shop-order/shop-order?nav=已支付')">
							<span class="icon iconfont icon-zhifudingjin" :style='{"color":"#fff","lineHeight":"1","fontSize":"48rpx"}'></span>
							<view :style='{"color":"#fff","lineHeight":"2","fontSize":"24rpx"}' class="title">已支付</view>
						</view>
						<view class="list-item" :style='{"alignItems":"center","flex":"1","flexDirection":"column","display":"flex","height":"auto"}' @tap="onPageTap('../shop-order/shop-order?nav=已完成')">
							<span class="icon iconfont icon-yanziwancheng" :style='{"color":"#fff","lineHeight":"1","fontSize":"48rpx"}'></span>
							<view :style='{"color":"#fff","lineHeight":"2","fontSize":"24rpx"}' class="title">已完成</view>
						</view>
						<view class="list-item" :style='{"alignItems":"center","flex":"1","flexDirection":"column","display":"flex","height":"auto"}' @tap="onPageTap('../shop-order/shop-order?nav=已取消')">
							<span class="icon iconfont icon-quxiao19" :style='{"color":"#fff","lineHeight":"1","fontSize":"48rpx"}'></span>
							<view :style='{"color":"#fff","lineHeight":"2","fontSize":"24rpx"}' class="title">已取消</view>
						</view>
						<view class="list-item" :style='{"alignItems":"center","flex":"1","flexDirection":"column","display":"flex","height":"auto"}' @tap="onPageTap('../shop-order/shop-order?nav=已退款')">
							<span class="icon iconfont icon-fanhui13" :style='{"color":"#fff","lineHeight":"1","fontSize":"48rpx"}'></span>
							<view :style='{"color":"#fff","lineHeight":"2","fontSize":"24rpx"}' class="title">已退款</view>
						</view>
						<!-- <view class="list-item" :style='{"alignItems":"center","flex":"1","flexDirection":"column","display":"flex","height":"auto"}' @tap="onPageTap('../shop-order/shop-order?nav=已发货')">
							<span class="icon iconfont icon-menu16" :style='{"color":"#333","lineHeight":"1","fontSize":"36rpx"}'></span>
							<view :style='{"color":"#333","lineHeight":"2","fontSize":"24rpx"}' class="title">已发货</view>
						</view> -->
					</view>
				</view>
				
				<view :style='{"width":"100%","height":"auto"}'>
					<view :style='{"width":"100%","padding":"0 20rpx","background":"black","justifyContent":"space-between","display":"none","height":"auto"}'>
						<view :style='{"color":"#fff","fontSize":"28rpx","lineHeight":"72rpx"}'>我的服务</view>
					</view>
					<view class="me-menu-view">
						<block v-for="item in menuList" v-bind:key="item.roleName">
							<block v-if="role==item.roleName" v-bind:key="menu.menu" v-for="(menu,index) in item.backMenu">
								<block v-bind:key="child.tableName" v-for=" (child,sort) in menu.child">
									<view class="me-menu-item" v-if="child.tableName!='orders/已发货' && child.tableName!='orders/已退款' &&child.tableName!='orders/已取消' && child.tableName!='orders/未支付' && child.tableName!='orders/已支付' && child.tableName!='orders/已完成' && child.tableName!='exampaper' && child.tableName!='examquestion'&& child.tableName!='hasTranslate' " @tap="onPageTap('../'+child.tableName+'/list?userid='+user.id+'&menuJump='+child.menuJump)" hover-class="hover">
										<view class="me-menu-icon" :class="child.appFrontIcon" :style="{'color': meMenuColor[index]}"></view>
										<view class="text">{{child.menu}}</view>
										<view class="icon iconfont icon-jinru"></view>
									</view>
								</block>
							</block>
						</block>
						<view v-if="user&&user.id" @tap="onPageTap('../shop-recharge/pay-confirm')" class="me-menu-item" hover-class="hover">
							<view class="cuIcon-moneybag me-menu-icon" :style="{'color': meMenuColor[7]}"></view>
							<view class="text">用户充值</view>
							<view class="icon iconfont icon-jinru"></view>
						</view>
						<view v-if="user&&user.id" @tap="couponClick" class="me-menu-item" hover-class="hover">
							<view class="cuIcon-remind me-menu-icon" :style="{'color': meMenuColor[4]}"></view>
							<view class="text">我的优惠券</view>
							<view class="icon iconfont icon-jinru"></view>
						</view>
						<view v-if="user&&user.id" @tap="onPageTap('../shop-address/shop-address')" class="me-menu-item" hover-class="hover">
							<view class="cuIcon-selection me-menu-icon" :style="{'color': meMenuColor[6]}"></view>
							<view class="text">我的地址</view>
							<view class="icon iconfont icon-jinru"></view>
						</view>
						<view v-if="user&&user.id" @tap="passwordShow()" class="me-menu-item" hover-class="hover">
							<view class="cuIcon-lock me-menu-icon" :style="{'color': meMenuColor[6]}"></view>
							<view class="text">修改密码</view>
							<view class="icon iconfont icon-jinru"></view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 推荐 -->
			<view class="recommend" :style='{"width":"100%","margin":"20rpx 0 0 0","background":"none","height":"auto"}' v-if="recommendList.length">
				<view :style='{"padding":"0 0 0 60rpx","color":"#000","background":"url(http://codegen.caihongy.cn/20241213/cc4d3e7e15444fbc9d9fc1032b6a41f6.png) center center/100% 100% no-repeat","width":"340rpx","fontSize":"28rpx","lineHeight":"60rpx","fontWeight":"bold","height":"80rpx"}'>为你推荐</view>
				<view :style='{"padding":"20rpx 0 0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"space-between","height":"auto"}'>
					<view v-for="(item,index) in recommendList" :key="index" :style='{"padding":"20rpx","margin":"0 0 20rpx","borderRadius":"20rpx","flexWrap":"wrap","background":"#fff","flexDirection":"column","display":"flex","width":"48%","justifyContent":"center","height":"160rpx"}' @click="recommendDetail(item.id)">
						<image :style='{"width":"120rpx","objectFit":"cover","borderRadius":"20rpx","display":"block","height":"120rpx"}' :src="item[recommendPicture]?(baseUrl + item[recommendPicture].split(',')[0]):''"></image>
						<view :style='{"padding":"0 8rpx","whiteSpace":"nowrap","overflow":"hidden","color":"#000","textAlign":"center","width":"calc(100% - 120rpx)","lineHeight":"40rpx","fontSize":"28rpx","textOverflow":"ellipsis"}'>{{item[recommendTitle]}}</view>
						<view :style='{"padding":"0 8rpx","whiteSpace":"nowrap","overflow":"hidden","color":"#2DAA90","textAlign":"center","width":"calc(100% - 120rpx)","lineHeight":"40rpx","fontSize":"24rpx","textOverflow":"ellipsis"}'>{{item.addtime}}</view>
					</view>
				</view>
			</view>
			<view style="width: 100%;height: 80px"></view>
		</view>
		<uni-popup class="popup-content" ref="couponPopup" type="bottom">
			<view class="couponList">
				<view class="coupon" v-for="(item,index) in couponList" :key="index" :class="(item.status=='已过期'||item.status=='已使用')?'coupons':''">
					<view class="coupon_left">
						<view class="name">{{item.name}}</view>
						<view class="name">满{{item.fullamount}}减{{item.discountamount}}</view>
						<view class="date">{{item.startime}}-{{item.endtime}}</view>
						<view class="date">{{item.couponnumber}}</view>
						<view class="date">{{item.remark}}</view>
						<view class="date1" v-if="item.shangjiazhanghao">仅限 {{item.shangjiazhanghao}} 商品使用</view>
						<view class="date1" v-else>全品类商品可使用</view>
					</view>
					<view class="coupon_right">
						<view class="price">￥<span class="num">{{item.discountamount}}</span></view>
		
					</view>
					<img v-if="item.status=='已过期'" class="no-coupon" src="@/static/coupon/no-coupon.png" alt="">
					<img v-if="item.status=='已使用'" class="no-coupon" src="@/static/coupon/finish-coupon.png" alt="">
				</view>
				<view class="noList" v-if="!couponList.length">
					暂无优惠券
				</view>
			</view>
		</uni-popup>
		<uni-popup class="popup-content" ref="passwordPopup" type="bottom">
			<view class="passwordForm">
				<view class="passwordInput">
					<input type="password" v-model="passwordForm.mima" placeholder="原密码" />
				</view>
				<view class="passwordInput">
					<input type="password" v-model="passwordForm.newmima" placeholder="新密码" />
				</view>
				<view class="passwordInput">
					<input type="password" v-model="passwordForm.newmima1" placeholder="确认密码" />
				</view>
				<view class="passwordBtnView">
					<button class="passwordBtn" @click="updatePassword">更新</button>
				</view>
			</view>
		</uni-popup>
	</view>
</template>
<script>
	import menu from '@/utils/menu'
	export default {
		data() {
			return {
				isH5Plus: true,
				user: {},
				tableName:'',
				role: '',
				menuList: [],
				iconArr: [
					'cuIcon-same',
					'cuIcon-deliver',
					'cuIcon-evaluate',
					'cuIcon-shop',
					'cuIcon-ticket',
					'cuIcon-cascades',
					'cuIcon-discover',
					'cuIcon-question',
					'cuIcon-pic',
					'cuIcon-filter',
					'cuIcon-footprint',
					'cuIcon-pulldown',
					'cuIcon-pullup',
					'cuIcon-moreandroid',
					'cuIcon-refund',
					'cuIcon-qrcode',
					'cuIcon-remind',
					'cuIcon-profile',
					'cuIcon-home',
					'cuIcon-message',
					'cuIcon-link',
					'cuIcon-lock',
					'cuIcon-unlock',
					'cuIcon-vip',
					'cuIcon-weibo',
					'cuIcon-activity',
					'cuIcon-friendadd',
					'cuIcon-friendfamous',
					'cuIcon-friend',
					'cuIcon-goods',
					'cuIcon-selection'
				],
				recommendList: [],
				recommendTable: '',
				recommendTitle: '',
				recommendPicture: '',
				// 优惠券
				couponList: [],
				passwordForm: {
					mima: '',
					newmima: '',
					newmima1: '',
				},
				meMenuColor: '#409eff,#67c23a,#909399,#e6a23c,#f56c6c,#356c6c,#351c6c,#f093a9,#a7c23a,#104eff,#10441f,#a21233,#503319'.split(',')
			};
		},
		computed: {
			baseUrl() {
				return this.$base.url;
			},
		},
		async onLoad(){
			let menus = menu.list();
			this.menuList = menus;
			this.meMenuColor = this.meMenuColor.sort(()=> {
				return (0.5-Math.random());
			});
		},
		async onShow(){
            uni.removeStorageSync("useridTag");
			this.role = uni.getStorageSync("appRole");
			await this.getSession()
			this.getThumbsup()
			this.getStoreup()
			this.tableName = uni.getStorageSync("nowTable");
			let menus = menu.list();
			this.menuList = menus;
			this.getRecommendList()
			this.$forceUpdate()
		},
		methods: {
			async getSession(){
				let table = uni.getStorageSync("nowTable");
				let res = await this.$api.session(table);
				this.user = res.data;
				this.$forceUpdate()
			},
			async getThumbsup(){
				if(!this.user||!this.user.id){
					return false
				}
				let res = await this.$api.page('storeup',{userid: this.user.id,type: 21})
				this.user.thumbsnum = Number(res.data.total)
				this.$forceUpdate()
			},
			async getStoreup(){
				if(!this.user||!this.user.id){
					return false
				}
				let res = await this.$api.page('storeup',{userid: this.user.id,type: 1})
				this.user.storenum = Number(res.data.total)
				this.$forceUpdate()
			},
			async couponClick() {
				let res = await this.$api.page('mycoupon')
				for (let x in res.data.list) {
					if (!this.isBetweenTime(this.$utils.getCurDateTime(), [res.data.list[x].startime, res.data.list[x]
							.endtime
						]) && res.data.list[x].status != '已使用' && res.data.list[x].status !=
						'已过期') {
						res.data.list[x].status = '已过期'
						await this.$api.update('mycoupon', res.data.list[x])
					} else if (this.isBetweenTime(this.$utils.getCurDateTime(), [res.data.list[x]
							.startime, res.data.list[x].endtime
						]) && res.data.list[x].status != '已使用' && res.data.list[x].status !=
						'可使用') {
						res.data.list[x].status = '可使用'
						await this.$api.update('mycoupon', res.data.list[x])
					}
				}
				this.couponList = res.data.list
				this.$refs.couponPopup.open()
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
			loginClick(){
				uni.navigateTo({
					url: '../login/login'
				});
			},
			onPageTap(url) {
                uni.setStorageSync("useridTag",1);
				uni.navigateTo({
					url: url,
					fail: function() {
						uni.switchTab({
							url: url
						});
					}
				});
			},
			hasTranslate(){
				for(let x in this.menuList){
					if(this.menuList[x].roleName == this.role){
						for(let i in this.menuList[x].backMenu){
							if(this.menuList[x].backMenu[i].tableName=='hasTranslate'){
								return true
							}
						}
					}
				}
				return false
			},
			async getRecommendList(){
				let params = {
					page: 1,
					limit: 4
				}
				let res;
				if(uni.getStorageSync("appUserid")) {
					res = await this.$api.recommend2('nongchanpin', params);
				} else {
					res = await this.$api.recommend('nongchanpin', params);
				}
				this.recommendList = res.data.list
				this.recommendTable = 'nongchanpin'
				this.recommendTitle = 'chanpinmingcheng'
				this.recommendPicture = 'tupian'
				this.$forceUpdate()
			},
			recommendDetail(id) {
				uni.navigateTo({
					url: `../${this.recommendTable}/detail?id=${id}`,
					fail: function() {
						uni.switchTab({
							url: `../${this.recommendTable}/detail?id=${id}`
						});
					}
				});
			},
			passwordShow() {
				this.passwordForm = {
					mima: '',
					newmima: '',
					newmima1: '',
				}
				this.$forceUpdate()
				this.$refs.passwordPopup.open()
			},
			async updatePassword() {
				if (this.passwordForm.mima == ''){
					this.$utils.msg('原密码不能为空')
					return false
				}
				if (this.passwordForm.newmima == ''){
					this.$utils.msg('新密码不能为空')
					return false
				}
				if (this.passwordForm.newmima1 == ''){
					this.$utils.msg('确认密码不能为空')
					return false
				}
				let password = ''
				if (this.user.mima) {
					password = this.user.mima;
				} else if (this.user.password) {
					password = this.user.password;
				}
				if (this.tableName == 'yonghu') {
				}
				let newpassword = this.passwordForm.mima
				if(password != newpassword){
					this.$utils.msg('原密码不正确')
					return false
				}
				if (this.passwordForm.newmima != this.passwordForm.newmima1){
					this.$utils.msg('两次密码不一致')
					return false
				}
				if (this.passwordForm.mima == this.passwordForm.newmima){
					this.$utils.msg('新密码与原密码相同！')
					return false
				}
				this.user.password = this.passwordForm.newmima
				this.user.mima = this.passwordForm.newmima
				await this.$api.update(this.tableName,this.user)
				this.$utils.msg('修改密码成功,下次登录系统生效')
				this.$refs.passwordPopup.close()
				this.getSession()
			},
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		height: calc(100vh - 94px);
		box-sizing: border-box;
	}
	.couponList {
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		background: #fff;
		padding: 20px 20px 120px;
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
			position: relative;
			.no-coupon {
				position: absolute;
				width: 80px;
				height: 80px;
				right: 20px;
				top: 20px;
			}
	
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
		.coupons {
			filter: grayscale(100%);
		}
		  
		.noList {
			color: #9e9e9e;
			width: 100%;
			text-align: center;
			padding: 60px 0;
		}
	}
	.passwordForm {
		width: 100%;
		padding: 10px 20px 60px;
		background: #fff;
	
		.passwordInput {
			background: #eee;
			margin: 0 0 10px;
			padding: 0 10px;
	
			input {
				height: 36px;
				line-height: 36px;
				font-size: 14px;
			}
		}
	
		.passwordBtnView {
			width: 100%;
			display: flex;
			align-items: center;
			justify-content: center;
			padding: 10px 0;
	
			.passwordBtn {
				padding: 0 20px;
				width: auto;
				height: 20px;
				line-height: 20px;
				font-size: 14px;
				color: #fff;
				background: #007aff;
				border: none;
				border-radius: 5px;
	
			}
		}
	}
	.me-menu-view {
		border-radius: 20rpx;
		padding: 0 0 30rpx;
		margin: 30rpx 0;
		background: #fff;
		display: flex;
		width: 100%;
		flex-wrap: wrap;
		height: auto;
		.me-menu-item {
			padding: 30rpx 0 0;
			flex-direction: column;
			display: flex;
			width: 25%;
			align-items: center;
			height: auto;
			.me-menu-icon {
				color: rgba(0, 186, 189, 1);
				font-size: 52rpx;
				line-height: 1;
			}
			.text {
				padding: 0 20rpx;
				color: #000;
				width: 100%;
				font-size: 28rpx;
				line-height: 60rpx;
				text-align: center;
			}
			.icon {
				color: #999;
				display: none;
				width: 28rpx;
				font-size: 28rpx;
				line-height: 28rpx;
			}
		}
	}
</style>
