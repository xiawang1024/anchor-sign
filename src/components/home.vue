<template>
  <div class="home">
    <div class="hd"></div>
    <div class="main-one" :class="{ishidden:isHidden}">
		<div class="text" >
			<p class="param">
				进入新时代，培训掀高潮，好戏连台唱！ 
			</p>
			<p class="param">
				2017年11月23日上午，常智明总工程师将应主持人培训中心邀请，担任“主播论坛”2017第6期主讲嘉宾，以《科技发展对传媒的挑战与河南广播的融媒实践》为题，解读我台如何规划新技术平台，搭建政策框架，引领我台声音产品的发展，推动传统广播与新媒体共生共荣。 
			</p>
			<p class="param">
				请全台除当时值班以外的播音员、主持人按时参加；也欢迎各频率、部门的采编制作和新媒体运营管理人员一起学习！
			</p>
			<p class="param">
				以实际行动贯彻落实十九大精神。愿您在2017年最后一期主播论坛上，收获新知识、新技术、新理念、新惊喜！ 
			</p>
			<p class="param">
				时间：2017年11月23日上午9:00——12:00 
			</p>
			<p class="param">
				地点：广播大厦24楼会议室。 
			</p>
		</div>
		<button class="btn" @click="goSubmit">报名参加</button>
    </div>
	<div class="main-two animated fadeIn" :class="{ishidden:!isHidden}">
		<div class="form">
			<p class="label">
				<label for="name">姓名</label>
				<input type="text" id="name" v-model="name"> 
			</p>
			<p class="label">
				<label for="name">电话</label>
				<input type="tel" id="name" v-model="mobile">
			</p>
			<p class="label">
				<label for="name">部门</label>
				<input type="text" id="name" v-model="unit">
			</p>
		</div>
		<button class="submit" @click="postUser" :disabled="disabled">提交</button>
	</div>
	<div class="toast" v-html='msg' :class="{isshow:!isShow}">

	</div>
  </div>
</template>

<script>
import { postUserInfo, checkOpenId } from 'api/index'
export default {
  name: 'home',
  data () {
    return {
		isHidden:false,
		name:'',
		unit:'',
		mobile:'',
		openId:'',
		disabled:false,
		msg:'<h1>报名成功（请准时参加）</h1><p>时间：11月23日上午9:00</p><p>地点：广播大厦24楼会议室</p>',
		isShow:false
    }
  },
  created() {
	  this.openId = this._getQueryString('openId')
	  console.log('------------------------------------');
	  console.log(this.openId);
	  console.log('------------------------------------');
  },
  methods:{
		goSubmit() {
			this.isHidden = true
		},
	  	postUser() {
			if(!this.name){
				
				this.msg='请填写姓名'
				this.isShow = true
				setTimeout(() => {
					this.isShow = false
					this.$nextTick(() => {

					})
				},1500)
				return 
			}
			if(!this.mobile){
				this.msg='请填写手机号'
				this.isShow = true
				setTimeout(() => {
					this.isShow = false
					this.$nextTick(() => {

					})
				},1500)
				return 
			}
			if(!this._checkPhone(this.mobile)) {
				this.msg='请填写正确的手机号'
				this.isShow = true
				setTimeout(() => {
					this.isShow = false
					this.$nextTick(() => {

					})
				},1500)
				return 
			}
			if(!this.unit){
				this.msg='请填写部门'
				this.isShow = true
				setTimeout(() => {
					this.isShow = false
					this.$nextTick(() => {

					})
				},1500)
				return 
			}			
			postUserInfo(this.name, this.unit, this.mobile, this.openId).then((res) => {
				this.disabled = true;
				let data = res.data
				if(data.status == 1) {	
					this.msg='<h1>报名成功（请准时参加）</h1><p>时间：11月23日上午9:00</p><p>地点：广播大厦24楼大会议室</p>'				
					this.isShow = true
					setTimeout(() => {
						this.disabled = false
						this.isShow = false
                        this.$nextTick(() => {

                        })
                    },5000)
				}else{
					this.msg = '<h1>报名失败：</h1><p>该微信已经报过名！</p>'
					this.isShow = true
					setTimeout(() => {
						this.disabled = false
                        this.isShow = false
                        this.$nextTick(() => {

                        })
                    },3000)
				}
				
			}).catch(() => {
				this.msg = '<h1>报名失败：</h1><p>网络错误，请再试！</p>'
					this.isShow = true
					setTimeout(() => {
						this.disabled = false
                        this.isShow = false
                        this.$nextTick(() => {

                        })
                    },3000)
			})
            
		},
		_getQueryString(name) {
			let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
			let r = window.location.search.substr(1).match(reg);
			if (r != null) return unescape(r[2]);
			return null;
		},
		_checkPhone(phone) { 
			if(!(/^1[34578]\d{9}$/.test(phone))){                 
				return false; 
			}else{
				return true
			}
		}
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.main-one.ishidden,.main-two.ishidden
	display none
.home
	position absolute
	top 0
	right 0
	bottom 0
	left 0
	width 750px
	height 100%
	background url('./bg.jpg') center center no-repeat
	background-size cover
	.hd
		width 100%
		height 296px
		margin-top 60px
		background url('./hd.png') center center no-repeat
		background-size 8.8rem auto
	.main-one
		width 100%
		box-sizing border-box
		.text
			width 100%
			height 600px
			overflow auto
			-webkit-overflow-scrolling: touch
			padding 0 40px
			color #ffffff
			box-sizing border-box
			.param
				margin-bottom 10px
				text-indent 2em
				line-height 2
				font-size 26px
		.btn
			position absolute
			bottom 75px
			width 250px
			height 80px
			border none 
			outline none 
			background: orange			
			margin-left 50%
			transform translateX(-50%)
			font-size 30px
	.main-two
		width 100%
		box-sizing border-box
		.form
			width 545px
			margin 100px 0 0 102px 
			box-sizing border-box
			overflow hidden
			.label
				display flex
				width 100%
				height 78px
				margin-bottom 30px
				font-size 30px
				color #fff
				box-sizing border-box
				label
					align-self center
					width 72px
					text-align center
				input 
					margin-left 40px
					padding-left 15px
					flex 1
					border 1px solid #e5e5e5
					background none
					color #ffffff
		.submit
			margin-top 100px
			width 250px
			height 80px
			border none 
			outline none 
			background: orange			
			margin-left 50%
			transform translateX(-50%)
			font-size 30px
	.toast
		position absolute
		top 50%
		transform translateY(-100%)
		width 550px
		margin-left 100px
		padding 60px
		background rgba(0,0,0,0.85)
		line-height 2
		font-size 32px
		color #ffffff
		text-align center
		border-radius 8px
		border 1px solid orange
		box-sizing border-box
		&.isshow
			display none
</style>

