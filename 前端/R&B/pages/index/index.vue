<template>
	<view>
		<!-- 中国加油界面 -->
		<view class="welcome" style="overflow-y: hidden;" v-if="!isShowContent" :style="{'height':height}">
			<scroll-view scroll-y class="DrawerPage" :class="modalName=='viewModal'?'show':''">
				<uni-fab ref="fab" :pattern="pattern" :horizontal="horizontal" :vertical="vertical" :direction="direction"  @fabClick="showDrawer" Size="18px" Height="35px" Icon="send"/>
				<view :style="{'height':height}" style="background:url('/h5/static/Search_BG.png') no-repeat center; background-size:cover; " >
					<!-- <image src="/static/Search_BG.png" mode="aspectFit" style="width:100%;height:100%; "  :style="[{animation: 'show 1s 1'}]"></image> -->
					<view>
						<image src="/static/Search_CNJY.png" @click="showContent();" mode="aspectFit" style="width: 90%;height:17%; position:absolute; left:calc(44rpx); top: calc(210rpx); border:#000 solid 0px;" :style="[{animation: 'show 8s 100 alternate linear'}]"></image>
						<!-- <image src="/static/Search_CNJY.png" class="animation-slide-top" mode="aspectFit" style="width: 90%;height:17%; position:absolute; left:calc(44rpx); top: calc(180rpx); border:#000 solid 0px;animationDelay: 0.4s" ></image> -->
					</view>
					<view>
						<image src="/static/Search_Button.png" class="animation-slide-top"  @click="showContent();" mode="aspectFit" style="width: 90%;height:17%; position:absolute; left:calc(44rpx); top: calc(550rpx); border:#000 solid 0px" :style="[{animation: 'show 1s 1'}]"></image>
					</view>
				</view>
			</scroll-view>
			
			<!-- 右侧抽屉 -->
			<view class="DrawerClose" :class="modalName=='viewModal'?'show':''" @tap="hideModal">
				<text class="cuIcon-pullright"></text>
			</view>
			
			<scroll-view scroll-y class="DrawerWindow" :class="modalName=='viewModal'?'show':''">
				<!-- 个人界面 -->
				<view class="header" style="margin-left: calc(40upx);">
					<text class="text-white text-bold" style="font-size: 56upx;" >团队介绍</text>
					<!-- </view> -->
				</view>
			
				<!-- list -->
				<view class="cu-list menu card-menu margin-top-xl margin-bottom-xl shadow-lg">
					
					<view class="cu-item arrow">
						<view class="content">
							<text class="text-black text-xl"><br/></text>
							<text class="cuIcon-form text-black"></text>
							<text class="text-black text-xl text-bold">北京大学</text>
							<text class="text-white text-xl text-bold"><br/>......</text>
							<text class="text-black text-xl text-bold">软件与微电子学院</text>
							<text class="text-white text-xl text-bold"><br/><br/>......</text>
							<!-- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本平台面向的受众为中老年群体，内容为养生方向的谣言鉴别和辟除，其中分为搜索界面，谣言列表界面和辟谣问答游戏三个界面，目前已实现基本的所有功能，后期将不断对数据使用自然语言处理进行获取分类，希望得到各位用户的认可～ -->
							<!-- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果您对我们的网站还满意的话，就在我们的 GitHub 点个 ⭐ 叭～   つ♡⊂ -->
							<text class="text-black text-xl">
								本平台由北京大学软件与微电子学院王可欣团队搭建而成，团队成员如下：
															
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;王可欣  周慧敏  程玄
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;陈鸿凯  江姗姗  徐康
								
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本平台为新型冠状病毒肺炎（ Corona Virus Disease 2019，COVID-19 ）科普平台，内容为有关本次新冠肺炎的谣言鉴别和辟除，其中分为搜索界面，谣言列表界面和辟谣问答游戏三个界面，目前已实现基本的所有功能，后期将不断对数据使用自然语言处理进行获取分类，希望得到各位用户的认可～
								
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;感谢您的浏览～   つ♡⊂
								
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;👍 👍 👍 👍 👍 👍 👍
								&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;——————————
								
							</text>
							<text class="text-white text-xl text-bold"><br/>.</text>
							<text class="text-white text-xl text-bold"><br/>.</text>
						</view>
					</view>			
							
				</view>			

			</scroll-view>
		<!-- end -->
		</view>
		
		<!--搜索栏-->
		<view v-if="isShowContent">
			<view class="search-box nav fixed" style="background-color: #ffffff; box-shadow:none" :style="{'width':width}">
				<mSearch ref="input" id="search-box" class="mSearch-input-box" :mode="2" button="inside" :placeholder="defaultKeyword" @search="doSearch" @input="inputChange" @confirm="doSearch(false)"  v-model="keyword" @getFocus="showKeywordList" @return="hideContent"></mSearch>
			</view>
			
			<view style="height: 110upx; background: #ffffff; bottom: 10upx;"></view>
			<view class="search-keyword" @touchstart="blur" v-if="isShowKeywordList">
				<scroll-view class="keyword-box" v-show="isShowKeywordList" scroll-y>
					<view class="keyword-block" v-if="oldKeywordList.length>0">
						<view class="keyword-list-header">
							<view>历史搜索</view>
							<view>
								<image @tap="oldDelete" src="/static/HM-search/delete.png"></image>
							</view>
						</view>
						<view class="keyword">
							<view v-for="(keyword,index) in oldKeywordList" @tap="doSearch(keyword)" :key="index">{{keyword}}</view>
						</view>
					</view>
					
					<view class="keyword-block">
						<view class="keyword-list-header">
							<view>热门搜索</view>
							<view>
								<image @tap="hotToggle" :src="'/static/HM-search/attention'+forbid+'.png'"></image>
							</view>
						</view>
						<view class="keyword" v-if="forbid==''">
							<view v-for="(keyword,index) in recKeyWordList" @tap="doSearch(keyword)" :key="index">{{keyword}}</view>
						</view>
						<view class="hide-hot-tis" v-else>
							<view>当前热门搜索已隐藏</view>
						</view>
					</view>
				</scroll-view>
			</view>
			
			<!--新闻列表,只有有数据的时候才显示-->
			<view class="uni-list" v-if="listData.length >0">
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(value,key) in listData" :key="key"
					@click="goDetail(value)">
					<view class="uni-media-list">
						<image class="uni-media-list-logo" :src="value.avatar" v-if="value.avatar!=null"></image>
						<!--显示默认图片-->
						<image class="uni-media-list-logo" src="/static/avatar.png" v-if="value.avatar==null"></image>
						<view class="uni-media-list-body">
							<!-- 设置布局 -->
							<view class="uni-media-list-text-top">
								<!--标题-->
								<view>{{value.title}}</view>
								<view>
								<image v-if='value.result=="假" || value.type=="假"' src="/static/fake_logo.png" style="position:absolute;width: 100rpx; height: 100rpx; right: 20rpx; top: 20rpx;"></image>
								<image v-if='value.result=="真" || value.type=="真"' src="/static/true_logo.png" style="position:absolute;width: 100rpx; height: 100rpx; right: 20rpx; top: 20rpx;"></image>
								<image v-if='value.result=="疑" || value.type=="论"' src="/static/doubt_logo.png" style="position:absolute;width: 100rpx; height: 100rpx; right: 20rpx; top: 20rpx;"></image>
								</view>
									<!--标签-->
									<!-- <text v-if='value.result=="假" || value.type=="假"' class='cu-tag text-white text-bold ' style="background-color: #910000; font-size: 24upx; padding: 0 21upx; height: 40upx;"> -->
										<!-- {{value.type}} -->
									<!-- </text> -->
									<!-- <text v-if='value.result=="真"|| value.type=="真"' class='cu-tag text-white text-bold bg-green ' style="font-size: 24upx; padding: 0 21upx; height: 40upx;">
										{{'确认属实！！'}}
									</text>
									<text  v-if='value.result=="疑"|| value.type=="论"' class='cu-tag text-white text-bold bg-yellow' style="font-size: 24upx; padding: 0 21upx; height: 40upx;">
										{{'尚未定论️'}}
									</text> -->
							</view>
							<!-- 谣言时间和来源 -->
							<view class="uni-media-list-text-bottom">
								<text>{{value.date}}</text>
								<text>{{value.platform}}</text>
							</view>
						</view>
					</view>
				</view>
				<uni-load-more :status="status"></uni-load-more>
			</view>
				
		</view>
		<!--搜索结果为空 跳出弹窗-->
		<uni-popup ref="popupEmpty" type="center" :mask-click="false" :animation="true">
			<view  class="uni-tip">
					<view class="uni-tip-title text-xl">
						暂无相关搜索结果<br/>请尝试更换您的关键词<br/>つ♡⊂<br/>———————————
					</view>
					<!-- <view class="uni-tip-content">
						找不到结果
					</view>		 -->
					<view class="uni-tip-group-button">
							<text class="uni-tip-button text-xl" @click="clearInput">好的👌</text>
					</view>												
			</view>
		</uni-popup>	
			
	</view>
</template>

<script>
	// 在微信开发者工具自动播放，本地网络没有反应，怀疑是 iOS 端播不了 or h5 播不了
	const innerAudioContext = uni.createInnerAudioContext()
	innerAudioContext.autoplay = false
	innerAudioContext.loop = true
	innerAudioContext.src = '/static/bgm.mp3'
	import uniPopup from "@/components/uni-popup/uni-popup.vue"
    var dateUtils = require('../../common/util.js').dateUtils;
    export default {
		components: {
			uniPopup	
		},
        data() {
            return {
				isShowContent:false,//显示欢迎界面
				swiperHeight: '1600upx',//
				height:'',
				modalName: null,
				directionStr: '水平',
				horizontal: 'right',
				vertical: 'bottom',
				direction: 'vertical',
				pattern: {
					color: '#7A7E83',
					backgroundColor: '#fff',
					selectedColor: '#690000',
					buttonColor: '#5e0000'
				},
                listData: [],
                last_id: 0,
				reload: false,
				//搜索框相关
				defaultKeyword: "",
				keyword: "",
				oldKeywordList: [],
				hotKeywordList: ['123','456'],
				keywordList: [],
				forbid: '',
				forbid_news: '',
				isShowKeywordList: true,
				width:'',
				status:"more", //最下面显示加载状态
				recKeyWordList: [],
				hotKeyWordQuesList: ['排毒保健品吃多了肠子会变黑', '喝小分子水，能治心血管病', '喝磁化水可治疗结石病', '干细胞美容法', '补硒可以防癌', '灵芝可以治疗很多疾病', '口服胶原蛋白可以美容', 
									'佩戴法力藤钛圈可治疗疾病', '“玛卡”保健品可以提高性能力', '猴菇饼干有养胃功效', '羊胎素具有美肤的功效并且能有效抵抗衰老', '“抗氧化之王”虾青素的种种神奇功效', 
									'富氧水是最健康的饮用水', '某些保健药含激素过量食用导致股骨头坏死', '纯银戒指慢慢变黑是因为吸取了身体的毒素', '4种饮用水喝了可能会致癌', '长时间用手机贴脸打电话可能被“电辐烫伤”', 
									'喝茶能降低死亡率', '成年人需要时刻补充维生素D', '观看猫咪的视频能获得正能量', '去海边只需要抹防晒霜，不需要太阳镜', '节水最好的方式是素食', 
									'经常喝咖啡会让牙齿变黄，刷牙也刷不掉', '喝橙汁健脑，能提高记忆力', '锻炼时摄入的水分会影响运动员速度', '多吃蛋白质就能多长肌肉', '有挑战的工作能让大脑减慢衰老', 
									'经常上网会损伤记忆力', '儿童沙子玩具里含有害添加剂，威胁健康', '白头发会越拔越多', '家用消毒剂让儿童更易感染', '看1小时电视糖尿病风险增加3.4%', '辣椒可能导致胃癌', 
									'被父母高估的孩子更易自恋', '沉迷电视剧也是一种病', '吃苦瓜可降血糖', '喝可乐会导致体内电解质流失', '睡前饮酒有助于睡眠', '低头玩手机颈椎承重45斤', '掌上设备伤眼，黑暗中看易患黄斑变性', 
									'全粗粮饮食有助延长寿命', '抽烟会让你喝酒喝得更凶', '长跑锻炼容易引发关节炎，步行更有益', '多吃芹菜会减少精子数量', '用老铁壶煮水有特殊功效，有益健康', '断食并饮用果蔬汁可以排毒减肥、改善健康', 
									'睡觉时闻臭味有助于戒烟', '每天多用热水泡脚可以改善身体健康', '长吃肉会降低智商', '购物小票中含有双酚A，可通过接触渗入人体', '鸡汤对感冒有靠谱的疗效', '不叠被子可杀死螨虫有益健康', 
									'过滤嘴可以降低卷烟的健康危害', '拔智齿影响健康', '长时间带耳机可能造成听力损伤', '经常染发可能患重症血液病', '抠鼻孔让人更聪明', '水果要空腹食用，早上吃最佳'],
            }
		},
		onLoad() {
		    // this.getList();
		    this.init();
		},
		onReady() {
			var tempHeight = 800;
			var tempWidth = 800;
			var _me = this;
			uni.getSystemInfo({
				//获取手机屏幕高度信息，让swiper的高度和手机屏幕一样高                
				success: (res)=> {                   
					tempWidth = res.windowWidth;
					tempHeight = res.windowHeight;
					// console.log("屏幕可用高度 " + tempHeight);
					// _me.swiperHeight = tempHeight + 'px';
					// console.log("滑屏最后高度 " + _me.swiperHeight);
					this.width = tempWidth + 'px';
					this.height = tempHeight + 'px';			
				}
			});
									
		},
		onBackPress() {
			if (this.$refs.fab.isShow) {
				this.$refs.fab.close()
				return true
			}
			return false
		},
        onReachBottom() {
			if(this.isShowContent&&(!this.isShowKeywordList)){
				this.reload = true;
				this.getList();
			}
        },
        methods: {
			//显示随机热门搜索内容
			showRandomHotSearch:function(e){
				//打乱题目
				function randomsort(a, b) {
					return Math.random()>.5 ? -1 : 1;
					//用Math.random()函数生成0~1之间的随机数与0.5比较，返回-1或1
				}				
				this.hotKeyWordQuesList.sort(randomsort); //打乱数组
				this.recKeyWordList = this.hotKeyWordQuesList.slice(1,11);
			},
			// Drawer弹出
			showDrawer(){
				uni.showToast({
					title: '您打开了一个彩蛋',
					icon: 'none'
				})
				if( this.modalName == null ){
					this.modalName = 'viewModal';
				}else{
					this.modalName = null;
				}
				// console.log(this.modalName)
			},
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			//进入搜索页面
			showContent: function(e) {
				this.showRandomHotSearch();
				innerAudioContext.play();
				this.isShowContent = true;
			},
			//回到欢迎页面
            hideContent:function(msg){
				this.isShowContent = false;
				// console.log("hello World");
			},			
			//点击搜索框的函数
			showKeywordList:function(msg){
				this.showRandomHotSearch();
				this.isShowKeywordList = msg;
				//不显示新闻列表
				this.listData =[];
			},
			//流言列表的内容
            getList() {
				uni.showLoading({
					title: '加载中'
				});
				this.status = "loading"
                var data = {
					_sort:'date:DESC',//按照时间顺序排序
					_limit:10, //需要的字段名
					title_contains:this.keyword,
					_start : this.last_id
                };
                uni.request({
					url: 'http://120.79.197.140:1337/rumors',
					method: 'GET',
                    data: data,
                    success: (data) => {
						uni.hideLoading();
						this.status = "more"
                        if (data.statusCode == 200) {
							let list = data.data;
							if(list.length >0){
                                this.listData = this.reload ? this.listData.concat(list):list;
                                this.last_id = this.listData.length;
								this.reload = false;
								//已经没有更多结果
								if(list.length < 10){
									this.status= "noMore";
								}
                            } else {
								this.status= "noMore";
								if(this.reload == false){
									this.$refs.popupEmpty.open();
									this.$refs.input.isDisableInput = true;
								}
								
                            }
                        }
                    },
                    fail: (data, code) => {
                        console.log('fail' + JSON.stringify(data));
                    }
				})
			},
			//进入详情页面
            goDetail: function(e) {
                uni.navigateTo({
                    url: "../detail/detail?id=" + e.id
                })
            },
			init() {
				this.loadDefaultKeyword();
				this.loadOldKeyword();
				// this.loadHotKeyword();
			},
			blur(){
				uni.hideKeyboard()
			},
			//加载默认搜索关键字
			loadDefaultKeyword() {
				//定义默认搜索关键字，可以自己实现ajax请求数据再赋值,用户未输入时，以水印方式显示在输入框，直接不输入内容搜索会搜索默认关键字
				this.defaultKeyword = "请输入搜索信息";
			},
			//加载历史搜索,自动读取本地Storage
			loadOldKeyword() {
				uni.getStorage({
					key: 'OldKeys',
					success: (res) => {
						var OldKeys = JSON.parse(res.data);
						this.oldKeywordList = OldKeys;
					}
				});
			},
			//加载热门搜索
			loadHotKeyword() {
				//定义热门搜索关键字，可以自己实现ajax请求数据再赋值
				this.hotKeywordList = ['自来水煮过数次真的不能再次饮用吗？', '在外吃饭用开水烫碗筷真的有用吗？', '只吃素菜不吃荤菜真的可以减少健康问题吗？','睡眠时间越长越好吗？','热鸭梨水能抗癌吗？','缺维生素B2会致癌吗？','雪梨银耳能清肺？'];
			}, 
			//监听输入
			inputChange(event) {
				//兼容引入组件时传入参数情况
				var keyword = event.detail?event.detail.value:event;
				if (!keyword) {
					this.keywordList = [];
					this.isShowKeywordList = false;
					return;
				}
			},
			//高亮关键字
			drawCorrelativeKeyword(keywords, keyword) {
				var len = keywords.length,
					keywordArr = [];
				for (var i = 0; i < len; i++) {
					var row = keywords[i];
					//定义高亮#9f9f9f
					var html = row[0].replace(keyword, "<span style='color: #9f9f9f;'>" + keyword + "</span>");
					html = '<div>' + html + '</div>';
					var tmpObj = {
						keyword: row[0],
						htmlStr: html
					};
					keywordArr.push(tmpObj)
				}
				return keywordArr;
			},
			//顶置关键字
			setkeyword(data) {
				this.keyword = data.keyword;
			},
			//清除历史搜索
			oldDelete() {
				uni.showModal({
					content: '确定清除历史搜索记录？',
					success: (res) => {
						if (res.confirm) {
							console.log('用户点击确定');
							this.oldKeywordList = [];
							uni.removeStorage({
								key: 'OldKeys'
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			//热门搜索开关
			hotToggle() {
				this.forbid = this.forbid ? '' : '_forbid';
			},
			//谣言列表开关
			hotToggle_news() {
				this.forbid_news = this.forbid_news ? '' : '_forbid';
			},
			//执行搜索
			doSearch(key) {
				this.isShowKeywordList = false;
				key = key ? key : this.keyword ? this.keyword : this.defaultKeyword;
				// this.keyword = key;
				//清空上一次搜索结果
				this.listData = [];
				this.last_id = 0;
				this.saveKeyword(key); //保存为历史 
				this.keyword = key;
                // console.log(key+" a 为什么啊")
                this.getList();
			},
			//清楚搜索
			clearSearch(msg){
				this.isShowKeywordList = msg;
			},
			//保存关键字到历史记录
			saveKeyword(keyword) {
				uni.getStorage({
					key: 'OldKeys',
					success: (res) => {
						console.log(res.data);
						var OldKeys = JSON.parse(res.data);
						var findIndex = OldKeys.indexOf(keyword);
						if (findIndex == -1) {
							OldKeys.unshift(keyword);
						} else {
							OldKeys.splice(findIndex, 1);
							OldKeys.unshift(keyword);
						}
						//最多10个纪录
						OldKeys.length > 10 && OldKeys.pop();
						uni.setStorage({
							key: 'OldKeys',
							data: JSON.stringify(OldKeys)
						});
						this.oldKeywordList = OldKeys; //更新历史搜索
					},
					fail: (e) => {
						var OldKeys = [keyword];
						uni.setStorage({
							key: 'OldKeys',
							data: JSON.stringify(OldKeys)
						});
						this.oldKeywordList = OldKeys; //更新历史搜索
					}
				});
			},		
            //清除输入
            clearInput:function(e){
                this.keyword = "";
                // this.getList();
				this.$refs.popupEmpty.close();
				this.$refs.input.isDisableInput = false;
				this.isShowKeywordList = true;
			},
			switchBtn(hor, ver) {
				if (hor === 0) {
					this.direction = this.direction === 'horizontal' ? 'vertical' : 'horizontal'
					this.directionStr = this.direction === 'horizontal' ? '垂直' : '水平'
				} else {
					this.horizontal = hor
					this.vertical = ver
				}
				this.$forceUpdate()
			},

        },
    }
</script>

<style>
	@import "../../colorui/animation.css";	

    .uni-list {
        background-color: #FFFFFF;
        position: relative;
        width: 100%;
        display: flex;
        flex-direction: column;
    }

    .uni-list:after {
        position: absolute;
        z-index: 10;
        right: 0;
        bottom: 0;
        left: 0;
        height: 1px;
        content: '';
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
        background-color: #c8c7cc;
    }

    .uni-list::before {
        position: absolute;
        z-index: 10;
        right: 0;
        top: 0;
        left: 0;
        height: 1px;
        content: '';
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
        background-color: #c8c7cc;
    }

    .uni-list-cell {
        position: relative;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }

    .uni-list-cell-hover {
        background-color: #eee;
    }

    .uni-list-cell::after {
        position: absolute;
        z-index: 3;
        right: 0;
        bottom: 0;
        left: 30upx;
        height: 1px;
        content: '';
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
        background-color: #c8c7cc;
    }

    .uni-list .uni-list-cell:last-child::after {
        height: 0upx;
    }

    /* 图文列表 */
    .uni-media-list {
        padding: 22upx 30upx;
        box-sizing: border-box;
        display: flex;
        width: 100%;
        flex-direction: row;
    }

    .uni-navigate-right.uni-media-list {
        padding-right: 74upx;
    }

    .uni-pull-right {
        flex-direction: row-reverse;
    }

    .uni-pull-right>.uni-media-list-logo {
        margin-right: 0upx;
        margin-left: 20upx;
    }

    .uni-media-list-logo image {
        height: 100%;
        width: 100%;
    }


    .uni-media-list-text-bottom {
        width: 100%;
        line-height: 30upx;
        font-size: 26upx;
        color: #8f8f94;
    }

    .uni-media-list-logo {
        width: 180upx;
        height: 140upx;
        margin-right: 20upx;
    }

    .uni-media-list-body {
        display: flex;
        flex: 1;
        flex-direction: column;
        justify-content: space-between;
        align-items: flex-start;
        overflow: hidden;
        height: auto;
    }

    .uni-media-list-text-top {
        width: 100%;
        line-height: 36upx;
        font-size: 30upx;
        height: 74upx;
        font-size: 28upx;
        overflow: hidden;
    }

    .uni-media-list-text-bottom {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
	.popup-content {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		background-color: #fff;
		padding: 15px;
		font-size: 14px;
	}

	/* 提示窗口 */
	.uni-tip {
		/* #ifndef APP-NVUE */
		display: flex;
		flex-direction: column;
		/* #endif */
		padding: 15px;
		width: 300px;
		background-color: #fff;
		border-radius: 10px;
	}

	.uni-tip-title {
		margin-bottom: 10px;
		text-align: center;
		font-weight: bold;
		color: #333;
	}

	.uni-tip-content {
		/* padding: 15px;
 */
		color: #666;
	}

	.uni-tip-group-button {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		margin-top: 20px;
	}

	.uni-tip-button {
		flex: 1;
		text-align: center;
		color: #3b4144;
	}
	view{display:block;}
	.search-box {width:95%;background-color:rgb(255, 255, 255);padding:15upx 3%;display:flex;justify-content:space-between;}
	.search-box .mSearch-input-box{width: 100%;}
	.search-box .input-box {width:85%;flex-shrink:1;display:flex;justify-content:center;align-items:center;}
	.search-box .search-btn {width:15%;margin:0 0 0 2%;display:flex;justify-content:center;align-items:center;flex-shrink:0;font-size:28upx;color:#fff;background:linear-gradient(to right,#ff9801,#ff570a);border-radius:60upx;}
	.search-box .input-box>input {width:100%;height:60upx;font-size:32upx;border:0;border-radius:60upx;-webkit-appearance:none;-moz-appearance:none;appearance:none;padding:0 3%;margin:0;background-color:#ffffff;}
	.placeholder-class {color:#ffffff;}
	.search-keyword {width:100%;background-color:rgb(255, 255, 255);}
	.keyword-list-box {height:calc(100vh - 110upx);padding-top:10upx;border-radius:20upx 20upx 0 0;background-color:#fff;}
	.keyword-entry-tap {background-color:#eee;}
	.keyword-entry {width:94%;height:80upx;margin:0 3%;font-size:30upx;color:#333;display:flex;justify-content:space-between;align-items:center;border-bottom:solid 1upx #e7e7e7;}
	.keyword-entry image {width:60upx;height:60upx;}
	.keyword-entry .keyword-text,.keyword-entry .keyword-img {height:80upx;display:flex;align-items:center;}
	.keyword-entry .keyword-text {width:90%;}
	.keyword-entry .keyword-img {width:10%;justify-content:center;}
	.keyword-box {height:calc(100vh - 110upx);border-radius:20upx 20upx 0 0;background-color:#fff;}
	.keyword-box .keyword-block {padding:10upx 0;}
	.keyword-box .keyword-block .keyword-list-header {width:94%;padding:10upx 3%;font-size:27upx;color:#333;display:flex;justify-content:space-between;}
	.keyword-box .keyword-block .keyword-list-header image {width:40upx;height:40upx;}
	.keyword-box .keyword-block .keyword {width:94%;padding:3px 3%;display:flex;flex-flow:wrap;justify-content:flex-start;}
	.keyword-box .keyword-block .hide-hot-tis {display:flex;justify-content:center;font-size:28upx;color:#6b6b6b;}
	.keyword-box .keyword-block .keyword>view {display:flex;justify-content:center;align-items:center;border-radius:60upx;padding:0 20upx;margin:10upx 20upx 10upx 0;height:60upx;font-size:28upx;background-color:rgb(242,242,242);color:#6b6b6b;}
	.input {
		width: 100%;
		max-width: 100%;
		line-height: 800upx;
		height: 100upx;
		transition: all 0.2s linear;
		padding:20px;
	}
	.DrawerPage {
		position: fixed;
		width: 100vw;
		height: 100vh;
		left: 0vw;
		background-color: #f1f1f1;
		transition: all 0.4s;
	}
	
	.DrawerPage.show {
		transform: scale(0.9, 0.9);
		left: 85vw;
		box-shadow: 0 0 60upx rgba(0, 0, 0, 0.2);
		transform-origin: 0;
	}
	
	.DrawerWindow {
		position: absolute;
		width: 85vw;
		height: 100vh;
		left: 0;
		top: 0;
		transform: scale(0.9, 0.9) translateX(-100%);
		opacity: 0;
		pointer-events: none;
		transition: all 0.4s;
		padding: 100upx 0;
		background-color: #500200;
	}
	
	.DrawerWindow.show {
		transform: scale(1, 1) translateX(0%);
		opacity: 1;
		pointer-events: all;
	}
	
	.DrawerClose {
		position: absolute;
		width: 40vw;
		height: 100vh;
		right: 0;
		top: 0;
		color: transparent;
		padding-bottom: 30upx;
		display: flex;
		align-items: flex-end;
		justify-content: center;
		background-image: linear-gradient(90deg, rgba(0, 0, 0, 0.01), rgba(0, 0, 0, 0.6));
		letter-spacing: 5px;
		font-size: 50upx;
		opacity: 0;
		pointer-events: none;
		transition: all 0.4s;
	}
	
	.DrawerClose.show {
		opacity: 1;
		pointer-events: all;
		width: 15vw;
		color: #fff;
	}
	
	.DrawerPage .cu-bar.tabbar .action button.cuIcon {
		width: 64upx;
		height: 64upx;
		line-height: 64upx;
		margin: 0;
		display: inline-block;
	}
	
	.DrawerPage .cu-bar.tabbar .action .cu-avatar {
		margin: 0;
	}
	
	.DrawerPage .nav {
		flex: 1;
	}
	
	.DrawerPage .nav .cu-item.cur {
		border-bottom: 0;
		position: relative;
	}
	
	.DrawerPage .nav .cu-item.cur::after {
		content: "";
		width: 10upx;
		height: 10upx;
		background-color: currentColor;
		position: absolute;
		bottom: 10upx;
		border-radius: 10upx;
		left: 0;
		right: 0;
		margin: auto;
	}
	
	.DrawerPage .cu-bar.tabbar .action {
		flex: initial;
	}

</style>