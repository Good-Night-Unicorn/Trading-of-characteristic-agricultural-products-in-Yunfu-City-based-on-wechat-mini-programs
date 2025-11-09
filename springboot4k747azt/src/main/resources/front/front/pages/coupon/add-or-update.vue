<template>
<view class="content">
	<view :style='{"minHeight":"100vh","width":"100%","padding":"0","position":"relative","background":"#DCF7FF","height":"auto"}'>
		<form :style='{"width":"100%","padding":"0","background":"none","display":"block","height":"auto"}' class="app-update-pv">
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">名称</view>
				<input :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' :disabled="ro.name" v-model="ruleForm.name" placeholder="名称"  type="text"></input>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class=" select">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">券类型</view>
				<picker :disabled="ro.type" :style='{"width":"100%","flex":"1","height":"auto"}' @change="typeChange" :value="typeIndex" :range="typeOptions">
					<view :style='{"width":"100%","padding":"0px 20rpx","lineHeight":"80rpx","fontSize":"28rpx","color":"#1CC3EE"}' class="uni-input">{{ruleForm.type?ruleForm.type:"请选择券类型"}}</view>
				</picker>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">满额</view>
				<input :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' :disabled="ro.fullamount" v-model.number="ruleForm.fullamount" placeholder="满额" type="digit"></input>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">优惠额</view>
				<input :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' :disabled="ro.discountamount" v-model.number="ruleForm.discountamount" placeholder="优惠额" type="digit"></input>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">生效时间</view>
				<input :disabled="ro.startime" :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' v-model="ruleForm.startime" placeholder="生效时间" @tap="toggleTab('startime')"></input>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">过期时间</view>
				<input :disabled="ro.endtime" :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' v-model="ruleForm.endtime" placeholder="过期时间" @tap="toggleTab('endtime')"></input>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">备注</view>
				<input :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' :disabled="ro.remark" v-model="ruleForm.remark" placeholder="备注"  type="text"></input>
			</view>
			<view :style='{"padding":"12rpx 0","margin":"0 0 24rpx 0","borderColor":"#ccc","alignItems":"center","borderWidth":"0 0 2rpx 0","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}' class="">
				<view :style='{"width":"160rpx","padding":"0 20rpx 0 0","lineHeight":"80rpx","fontSize":"26rpx","color":"#727272","textAlign":"right"}' class="title">商户名称</view>
				<input :style='{"border":"0","padding":"0px 20rpx","margin":"0px","color":"rgb(0, 0, 0)","borderRadius":"8rpx","flex":"1","background":"rgba(255, 255, 255, 0)","fontSize":"28rpx","height":"80rpx"}' :disabled="ro.shangjiazhanghao" v-model="ruleForm.shangjiazhanghao" placeholder="商户名称"  type="text"></input>
			</view>
         
			
			<view :style='{"width":"100%","alignItems":"center","flexDirection":"column","justifyContent":"space-between","display":"flex","height":"auto"}' class="btn" >
				<button :style='{"border":"0","padding":"0px","margin":"0 0 20rpx","color":"rgb(255, 255, 255)","background":"#37E8DC","width":"60%","lineHeight":"80rpx","fontSize":"28rpx","height":"80rpx"}' @tap="onSubmitTap" class="bg-red">提交</button>
			</view>
		</form>

		<w-picker  mode="dateTime" step="1" :current="false" :hasSecond="false" @confirm="startimeConfirm" ref="startime" themeColor="#333333"></w-picker>
		<w-picker  mode="dateTime" step="1" :current="false" :hasSecond="false" @confirm="endtimeConfirm" ref="endtime" themeColor="#333333"></w-picker>
	</view>
</view>
</template>

<script>
	import wPicker from "@/components/w-picker/w-picker.vue";
	import xiaEditor from '@/components/xia-editor/xia-editor';
	import multipleSelect from "@/components/momo-multipleSelect/momo-multipleSelect";
	export default {
		data() {
			return {
				cross:'',
				ruleForm: {
				userid: '',
				name: '',
				type: '',
				fullamount: '',
				discountamount: '',
				startime: '',
				endtime: '',
				remark: '',
				shangjiazhanghao: '',
				},
				typeOptions: [],
				typeIndex: 0,
				// 登录用户信息
				user: {},
				ro:{
				   userid : false,
				   name : false,
				   type : false,
				   fullamount : false,
				   discountamount : false,
				   startime : false,
				   endtime : false,
				   remark : false,
				   shangjiazhanghao : false,
				},
				virtualPay: false,
			}
		},
		components: {
			wPicker,
			xiaEditor,
			multipleSelect,
		},
		computed: {
			baseUrl() {
				return this.$base.url;
			},



		},
		async onLoad(options) {
			if(options.virtualPay){
				this.virtualPay = true
			}
			let table = uni.getStorageSync("nowTable");
			// 获取用户信息
			let res = await this.$api.session(table);
			this.user = res.data;
			
			// ss读取
			this.ruleForm.shangjiazhanghao = this.user.shangjiazhanghao
			this.ro.shangjiazhanghao = true;

			this.ro.shangjiazhanghao = true;

			// 自定义下拉框值
			this.typeOptions = "满减券".split(',')

			// 如果有登录，获取登录后保存的userid
			this.ruleForm.userid = uni.getStorageSync("appUserid")
			if (options.refid) {
				// 如果上一级页面传递了refid，获取改refid数据信息
				this.ruleForm.refid = Number(options.refid);
				this.ruleForm.nickname = uni.getStorageSync("nickname");
			}
			// 如果是更新操作
			if (options.id) {
				this.ruleForm.id = options.id;
				// 获取信息
				res = await this.$api.info(`coupon`, this.ruleForm.id);
				this.ruleForm = res.data;
			}
			// 跨表
			this.cross = options.cross;
			if(options.cross){
				var obj = uni.getStorageSync('crossObj');
				for (var o in obj){
					if(o=='userid'){
						this.ruleForm.userid = obj[o];
						this.ro.userid = true;
						continue;
					}
					if(o=='name'){
						this.ruleForm.name = obj[o];
						this.ro.name = true;
						continue;
					}
					if(o=='type'){
						this.ruleForm.type = obj[o];
						this.ro.type = true;
						continue;
					}
					if(o=='fullamount'){
						this.ruleForm.fullamount = obj[o];
						this.ro.fullamount = true;
						continue;
					}
					if(o=='discountamount'){
						this.ruleForm.discountamount = obj[o];
						this.ro.discountamount = true;
						continue;
					}
					if(o=='startime'){
						this.ruleForm.startime = obj[o];
						this.ro.startime = true;
						continue;
					}
					if(o=='endtime'){
						this.ruleForm.endtime = obj[o];
						this.ro.endtime = true;
						continue;
					}
					if(o=='remark'){
						this.ruleForm.remark = obj[o];
						this.ro.remark = true;
						continue;
					}
					if(o=='shangjiazhanghao'){
						this.ruleForm.shangjiazhanghao = obj[o];
						this.ro.shangjiazhanghao = true;
						continue;
					}
				}
			}
			this.styleChange()
			this.$forceUpdate()
			if (uni.getStorageSync('raffleType') && uni.getStorageSync('raffleType') != null) {
				uni.removeStorageSync('raffleType')
				setTimeout(() => {
					this.onSubmitTap()
				}, 300)
			}
		},
		methods: {
			styleChange() {
				this.$nextTick(()=>{
					// document.querySelectorAll('.app-update-pv . .uni-input-input').forEach(el=>{
					//   el.style.backgroundColor = this.addUpdateForm.input.content.backgroundColor
					// })
				})
			},

			// 多级联动参数


			// 日长控件选择日期时间
			startimeConfirm(val) {
				console.log(val)
				this.ruleForm.startime = val.result;
				this.$forceUpdate();
			},
			// 日长控件选择日期时间
			endtimeConfirm(val) {
				console.log(val)
				this.ruleForm.endtime = val.result;
				this.$forceUpdate();
			},

			// 下拉变化
			typeChange(e) {
				this.typeIndex = e.target.value
				this.ruleForm.type = this.typeOptions[this.typeIndex]
			},


			getUUID () {
				return new Date().getTime();
			},
			async onSubmitTap() {
				let that = this
				//跨表计算判断
				var obj;
				if((!this.ruleForm.userid)){
					this.$utils.msg(`用户id不能为空`);
					return
				}
				if((!this.ruleForm.name)){
					this.$utils.msg(`名称不能为空`);
					return
				}
				if((!this.ruleForm.type)){
					this.$utils.msg(`券类型不能为空`);
					return
				}
				if((!this.ruleForm.fullamount)){
					this.$utils.msg(`满额不能为空`);
					return
				}
				if(this.ruleForm.fullamount&&(!this.$validate.isNumber(this.ruleForm.fullamount))){
					this.$utils.msg(`满额应输入数字`);
					return
				}
				if((!this.ruleForm.discountamount)){
					this.$utils.msg(`优惠额不能为空`);
					return
				}
				if(this.ruleForm.discountamount&&(!this.$validate.isNumber(this.ruleForm.discountamount))){
					this.$utils.msg(`优惠额应输入数字`);
					return
				}
				if((!this.ruleForm.startime)){
					this.$utils.msg(`生效时间不能为空`);
					return
				}
				if((!this.ruleForm.endtime)){
					this.$utils.msg(`过期时间不能为空`);
					return
				}
				//更新跨表属性
				var crossuserid;
				var crossrefid;
				var crossoptnum;
				if(this.cross){
					uni.setStorageSync('crossCleanType',true);
					var statusColumnName = uni.getStorageSync('statusColumnName');
					var statusColumnValue = uni.getStorageSync('statusColumnValue');
					if(statusColumnName!='') {
						if(!obj) {
							obj = uni.getStorageSync('crossObj');
						}
						if(!statusColumnName.startsWith("[")) {
							for (var o in obj){
								if(o==statusColumnName){
									obj[o] = statusColumnValue;
								}

							}
							var table = uni.getStorageSync('crossTable');
							await this.$api.update(`${table}`, obj);
						} else {
							   crossuserid=Number(uni.getStorageSync('appUserid'));
							   crossrefid=obj['id'];
							   crossoptnum=uni.getStorageSync('statusColumnName');
							   crossoptnum=crossoptnum.replace(/\[/,"").replace(/\]/,"");
						}
					}
				}
				if(crossrefid && crossuserid) {
					this.ruleForm.crossuserid=crossuserid;
					this.ruleForm.crossrefid=crossrefid;
					let params = {
						page: 1,
						limit:10,
						crossuserid:crossuserid,
						crossrefid:crossrefid,
					}
					let res = await this.$api.list(`coupon`, params);
					if (res.data.total >= crossoptnum) {
						this.$utils.msg(uni.getStorageSync('tips'));
						uni.removeStorageSync('crossCleanType');
						return false;
					} else {
				//跨表计算
						let oet = {}
						if(this.ruleForm.id){
							await this.$api.update(`coupon`, this.ruleForm);
						}else{
							oet = await this.$api.add(`coupon`, this.ruleForm);
						}

						this.$utils.msgBack('提交成功');
					}
				} else {
				//跨表计算
					let oet = {}
					if(this.ruleForm.id){
						await this.$api.update(`coupon`, this.ruleForm);
					}else{
						oet = await this.$api.add(`coupon`, this.ruleForm);
					}

					this.$utils.msgBack('提交成功');
				}
			},
			optionsChange(e) {
				this.index = e.target.value
			},
			bindDateChange(e) {
				this.date = e.target.value
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			toggleTab(str) {
				if(this.ro[str]){
					return false
				}
				this.$refs[str].show();
			},
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		min-height: calc(100vh - 44px);
		box-sizing: border-box;
	}
</style>
