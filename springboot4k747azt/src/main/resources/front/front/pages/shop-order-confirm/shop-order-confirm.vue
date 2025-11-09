<template>
	<view class="content">
		<form>
			<view v-if="seat!=1" @tap="onAddressTap" class="cu-form-group">
				<view class="title">地址</view>
				<view v-if="Object.keys(address).length">
					<view>
						{{address.name}} {{address.phone}}
					</view>
					<view>
						{{address.address}}
					</view>
				</view>
			</view>
			<view class="cu-form-group">
				<view class="title">购买清单</view>
			</view>
			<view v-for="(item,index) in orderGoods " v-bind:key="index" class="cu-form-group">
				<image class="avator" :src="baseUrl+item.picture" mode=""></image>
				<view class="title">
					<view>{{item.goodname}}</view>
					<view v-if="seat!=1">
						x{{item.buynumber}}
						<text v-if="type==1||type==3" style="margin-left: 30upx;color: red;">￥{{item.total}}</text>
						<text v-if="type==2" style="margin-left: 30upx;color: red;">{{item.total}}积分</text>
					</view>
					<view v-else>
						{{item.address}}号
						<text v-if="type==1" style="margin-left: 30upx;color: red;">￥{{item.total}}</text>
						<text v-if="type==2" style="margin-left: 30upx;color: red;">{{item.total}}积分</text>
					</view>
				</view>
			</view>
			<view class="cu-form-group">
				<view v-if="type==1||type==3" class="title">总价</view>
				<view v-if="type==2" class="title">总积分</view>
				<view>
					<text v-if="type==1||type==3">￥{{totalPrice}}</text>
					<text v-if="type==2">{{total}}积分</text>
				</view>
			</view>
			<view class="cu-form-group" v-if="type!=2">
				优惠券
				<input @tap="couponClick"  v-model="couponName" name="remark" style="margin-left: 30upx;  height: 60upx; font-size: 28rpx;text-align: right;"></input>
			</view>
			<view class="cu-form-group">
				备注
				<input v-model="remark" name="remark" style="margin-left: 30upx; border-bottom: 1px solid #999999; height: 60upx; font-size: 28rpx;"></input>
			</view>
		</form>
		<view class="padding" style="text-align: center;">
			<button  v-if="type==1" @tap="onSubmitTap()" class="bg-red lg">确认支付</button>
			<button  v-if="type==2" @tap="onSubmitTap()" class="bg-red lg">确认兑换</button>
		</view>
		<uni-popup class="popup-content" ref="couponPopup" type="bottom">
			<view class="couponList">
				<view class="coupon" v-for="(item,index) in couponList" :key="index" :class="!changeStatus(item)?'coupons':''">
					<view class="coupon_left">
						<view class="name">满{{item.fullamount}}减{{item.discountamount}}</view>
						<view class="date">{{item.startime}}-{{item.endtime}}</view>
						<view class="date">{{item.remark}}</view>
						<view class="date1" v-if="item.shangjiazhanghao">仅限 {{item.shangjiazhanghao}} 商品可使用</view>
						<view class="date1" v-else>全品类商品可使用</view>
					</view>
					<view class="coupon_right">
						<view class="price">￥<span class="num">{{item.discountamount}}</span></view>
						<view class="btn" @click="getCoupon(item)" v-if="item.status=='可使用'">{{changeStatus(item)?'立即使用':'不满条件'}}</view>
					</view>
					<img v-if="item.status=='已过期'" class="no-coupon" src="@/static/coupon/no-coupon.png" alt="">
					<img v-if="item.status=='已使用'" class="no-coupon" src="@/static/coupon/finish-coupon.png" alt="">
				</view>
				<view class="noList" v-if="!couponList.length">
					暂无优惠券
				</view>
				<view @click="clearCoupon" class="clearCoupon">不使用优惠券</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user: {},
				orderGoods: {},
				address: {},
				total: 0,
				type: 1,
				seat: 0,
				remark:'',
				couponList: [],
				nowCoupon: {},
				rightType:false,
				name: ''
			}
		},
		computed: {
			baseUrl() {
				return this.$base.url;
			},
			totalPrice(){
				let totalPrice = 0;
				if(this.seat == 1){
					totalPrice = this.total
				}else {
					if(this.orderGoods.length){
						this.orderGoods.forEach(item => {
							totalPrice += item.price * item.buynumber;
						});
					}
				}
				if(this.nowCoupon.id){
					totalPrice = totalPrice - this.nowCoupon.discountamount
				}
				return Number(totalPrice).toFixed(2);
			},
			couponName(){
				let name = ''
				if(this.nowCoupon.id){
					name = `满${this.nowCoupon.fullamount}减${this.nowCoupon.discountamount}`
				}
				return name?name: '选择优惠券'
			}
		},
		async onLoad(options) {
			this.type = options.type
			this.seat = options.seat
			// 获取订单信息，座位信息
			this.orderGoods = uni.getStorageSync('orderGoods');
			this.changeShangpin()
			if (this.seat != 1) {
				for (let i = 0; i < this.orderGoods.length; i++) {
					if(this.total==0) {
						this.total = parseFloat(this.orderGoods[i].price) * this.orderGoods[i].buynumber
					} else {
						this.total = parseFloat(this.total) + parseFloat(this.orderGoods[i].price) * this.orderGoods[i].buynumber	
					}
				}
			}else{
				this.total = parseFloat(this.orderGoods[0].total)
				this.address = this.orderGoods[0].address
			}
			this.changePrice()
			this.total= this.total.toFixed(2)
		},
		async onShow() {
			let table = uni.getStorageSync("nowTable");
			let res = await this.$api.session(table)
			this.user = res.data
			if(this.seat!=1){
				let address = uni.getStorageSync('address')
				if(address&&this.user.id==address.userid){
					this.address = uni.getStorageSync('address');
				}else{
					res = await this.$api.defaultAddress(this.user.id);
					if(res.data!=null){
						this.address = res.data
					}else {
						this.address = {}
					}
				}
			}
			
		},
		methods: {
		changeShangpin(){
			if(this.orderGoods.length==1){
				this.rightType = true
			}
			let name = ''
			this.orderGoods.forEach((item,index)=>{
				if(index==0){
					name = item.shangjiazhanghao
					this.rightType = true
				}else{
					if(item.shangjiazhanghao != name){
						this.rightType = false
					}
				}
			})
			if(this.rightType){
				this.name = name
			}
		},
			clearCoupon(){
				this.nowCoupon = {}
				this.changePrice()
				this.$forceUpdate()
				this.$refs.couponPopup.close()
			},
			async couponClick(){
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
			getCoupon(row){
				if (!this.changeStatus(row)) {
					return false
				}
				this.nowCoupon = row
				this.$refs.couponPopup.close()
				this.changePrice()
			},
			changeStatus(row) {
				let total = JSON.parse(JSON.stringify(this.totalPrice))
				if(this.nowCoupon.id){
					total = Number(total) + Number(this.nowCoupon.discountamount)
				}
				if (row.fullamount < Number(total)) {
					if (row.status=='可使用') {
						if(row.shangjiazhanghao&&this.rightType){
							if(row.shangjiazhanghao == this.name){
								return true
							}
						}else if(row.shangjiazhanghao&&!this.rightType){
							return false
							
						}else {
							return true
						}
					}
				}
				return false
			},
			changePrice(){
				let totalPrice = 0
				this.orderGoods.forEach(item => {
					if(this.seat==1){
						totalPrice = item.total
					}else {
						totalPrice += item.price * item.buynumber;
					}
				})
				let discountamount = 0
				let price = 0
				this.orderGoods.forEach(item => {
					if(this.seat==1){
						price = item.total
					}else {
						price = item.price * item.buynumber;
						
					}
					if(this.nowCoupon.id){
						discountamount = Number(((Number(price.toFixed(2)) / Number(totalPrice.toFixed(2))) * this.nowCoupon.discountamount).toFixed(2))
					}
					price = price - discountamount
					item.total = (price).toFixed(2)
				})
				this.$forceUpdate()
			},
			async onSubmitTap() {
				let _this = this;
				if(_this.seat!=1&&!_this.address.address){
					_this.$utils.msg(`请选择地址`);
					return;
				}
				let table = uni.getStorageSync("nowTable");
				let orderno = Number(_this.$utils.genTradeNo()) + 1
				
				uni.showModal({
					title: '提示',
					content: '是否确认支付',
					success: async function(res) {
						if (res.confirm) {
							// 订单类型
							let type = _this.type;
							let isSuccess = true ;
							let orderStatus = '已支付';
							if(type==1){
								if (_this.user.money - _this.totalPrice < 0) {
									_this.$utils.msg('余额不足，请先充值');
									isSuccess = false;
									orderStatus = '未支付';
									setTimeout(()=>{
										uni.switchTab({
											url: '../center/center'
										});
									},1500)
								}
							}else if(type==2){
								if (_this.user.jf - _this.totalPrice < 0) {
									_this.$utils.msg('积分不足，兑换商品失败');
									isSuccess = false;
									return;
								}
							}
							for (let i = 0; i < _this.orderGoods.length; i++) {
								// 获取数量 
								let rs2 = await _this.$api.info(`${_this.orderGoods[i].tablename}`, _this.orderGoods[i].goodid);
								// 单次限次
								if(rs2.data.onelimittimes>0){
									// 超过单次限次购买数量
									if(_this.orderGoods[i].buynumber>rs2.data.onelimittimes){
										 _this.$utils.msg(`${_this.orderGoods[i].goodname}超过可购买数量`);
										isSuccess = false
										break;
									}
								}
								// 如果库存小于当前购买数量
								if(rs2.data.alllimittimes<_this.orderGoods[i].buynumber){
									_this.$utils.msg(`${_this.orderGoods[i].goodname}已售罄`);
									isSuccess = false
									break;
								}else{
									if(_this.seat!=1){
										rs2.data.alllimittimes = rs2.data.alllimittimes - _this.orderGoods[i].buynumber;
										if(rs2.data.alllimittimes==0){
											rs2.data.onshelves = 0
										}
									}
								}
								let order =  {
									orderid: _this.$utils.genTradeNo(),
									tablename: _this.orderGoods[i].tablename,
									userid: _this.user.id,
									goodid: _this.orderGoods[i].goodid,
									goodname: _this.orderGoods[i].goodname,
									picture: _this.orderGoods[i].picture,
									buynumber: _this.orderGoods[i].buynumber,
									discountprice: _this.orderGoods[i].price,
									price: _this.orderGoods[i].price,
									total: _this.orderGoods[i].total,
									discounttotal: _this.orderGoods[i].total,
									type: type,
									remark: _this.remark,
									sfsh: '',
									role: table,
									address: _this.address.address,
									tel: _this.address.phone,
									consignee: _this.address.name,
									shangjiazhanghao:_this.orderGoods[i].shangjiazhanghao,
									goodtype: _this.orderGoods[i].goodtype,
									goodtype: _this.orderGoods[i].goodtype,
									status: orderStatus,
									orderno: orderno
								}
								if(_this.nowCoupon.id) {
									order.couponnumber = _this.nowCoupon.couponnumber
									order.discountamount = _this.nowCoupon.discountamount
									_this.nowCoupon.status = '已使用'
									await _this.$api.update('mycoupon', _this.nowCoupon);
								}
								if(_this.seat == 1){
									order['address'] = _this.address;
									let buynumber = _this.address.split(",").length;
									order['buynumber'] = buynumber;
									order['total'] = _this.orderGoods[i].total;
									order['discounttotal'] = _this.orderGoods[i].total;
									uni.setStorageSync('selectedType',true)
									// 更新座位
									if(rs2.data.selected) {
										rs2.data.selected =  rs2.data.selected  + "," + _this.orderGoods[i].activeSeat
									}else {
										rs2.data.selected =  _this.orderGoods[i].activeSeat
									}
								}
								await _this.$api.update(`${_this.orderGoods[i].tablename}`,  rs2.data);
								// 添加订单
								await _this.$api.add('orders', order);
								if (_this.orderGoods[i].id) {
									// 删除购物车对应的模块
									await _this.$api.del('cart', JSON.stringify([_this.orderGoods[i].id]))
									uni.removeStorageSync(`cart${_this.orderGoods[i].goodid}${_this.user.id}`)
								}
							}
							if(isSuccess){
								if(type==1){
									_this.$utils.msgBack('支付成功');
									// 减少余额
									_this.user.money = Number((Number(_this.user.money) - Number(_this.totalPrice)).toFixed(2));
									if(_this.user.jf||_this.user.jf==0){
										_this.user.jf = Number((Number(_this.user.jf) + Number(_this.totalPrice)).toFixed(2));
									}
									await _this.$api.update(table, _this.user);
									uni.setStorageSync('userSession',JSON.stringify(_this.user))
								}else if(type==2){
									_this.$utils.msgBack('兑换成功');
									// 减少积分
									_this.user.jf = Number((Number(_this.user.jf) - Number(_this.totalPrice)).toFixed(2));
									await _this.$api.update(table, _this.user);
									uni.setStorageSync('userSession',JSON.stringify(_this.user))
								}
							}
						}
					}
				});
			},
			onAddressTap() {
				this.$utils.jump('../shop-address/shop-address');
			}
		}
	}
</script>

<style lang="scss">
	.avator {
		width: 150upx;
		height: 150upx;
		margin: 20upx 0;
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
		.clearCoupon {
			margin: 0 auto;
			color: #666;
			font-size: 12px;
			
		}
	}
</style>
