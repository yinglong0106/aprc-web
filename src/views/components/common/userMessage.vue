<!-- <template>
  <el-dialog title="个人账号信息" width="900px" :visible.sync="isOpen" append-to-body destroy-on-close
    @close="beforeClose">
    <div class="title-bg mb10">账号信息</div>
    <el-descriptions :colon="false" class="margin-top" :column="1">
      <el-descriptions-item label-class-name="my-label" label="账号名称">18650767213</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="姓名">林都</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="联系电话">18650767213</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="注册日期">2021-09-18</el-descriptions-item>
    </el-descriptions>
    <div class="title-bg mb10">实名认证</div>
    <el-descriptions :colon="false" class="margin-top" :column="1">
      <el-descriptions-item label-class-name="my-label" label="姓名">18650767213</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="证件类型">林都</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="证件号码">18650767213</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="证件生效时间">2021-09-18</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="证件有效期限">2021-09-18</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="签发机关">2021-09-18</el-descriptions-item>
    </el-descriptions>
    <el-row>
      <div class="my-label mb10">证件证明</div>
      <div class="flex mb10">
        <el-image class="mr10" style="width: 100px; height: 100px" :src="url" :fit="fit"></el-image>
        <el-image class="mr10" style="width: 100px; height: 100px" :src="url" :fit="fit"></el-image>
        <el-image class="mr10" style="width: 100px; height: 100px" :src="url" :fit="fit"></el-image>
        <el-image class="mr10" style="width: 100px; height: 100px" :src="url" :fit="fit"></el-image>
      </div>
    </el-row>
    <div class="title-bg mb10">其他信息</div>
    <el-descriptions :colon="false" class="margin-top" :column="1">
      <el-descriptions-item label-class-name="my-label" label="角色类型">个人主体 > 退休人员</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="是否是引导专员">是</el-descriptions-item>
      <el-descriptions-item label-class-name="my-label" label="是否有引导专员">是</el-descriptions-item>
    </el-descriptions>
  </el-dialog>
</template>

<script>
  export default {
    name: "index",
    props: {
      row: {
        type: Object,
        default: () => {
          return {}
        }
      }
    },
    data() {
      return {
        isOpen: true,
        url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
        fit: 'fit'
      };
    },
    methods: {
      close() {
        this.isOpen = false
        this.$emit('close')
      },
      beforeClose() {
        this.$emit('close')
      },
    },
    onLoad() {},
  };
</script>

<style lang="scss" scoped>
  .title-bg {
    background-color: #F2F2F2;
    padding: 10px;
  }

  .my-label {
    width: 120px;
    font-weight: bold;
  }
</style> -->

<template>
	<el-dialog :title="title" :visible.sync="isOpen" width="700px" @close="beforeClose" append-to-body>
		<template v-if="pageType=='1'">
			<el-row class="">
				<div class="title-bg mb10">账号信息</div>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">账号名称</div>
					<div class="my-label">{{userInfo.username || userInfo.userName}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">姓名</div>
					<div class="my-label">{{userInfo.username || userInfo.userName}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">联系电话</div>
					<div class="my-label">{{userInfo.nation}}{{userInfo.phonenumber || userInfo.phonenumber}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">注册日期</div>
					<div class="my-label">{{userInfo.createTime || userInfo.registerTime}}</div>
				</el-row>
			</el-row>
			<!-- <el-row>
				<div class="title-bg mb10">实名认证</div>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">姓名</div>
					<div class="my-label">{{userInfo.realname}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">证件类型</div>
					<div class="my-label">{{userInfo.ID_type===''?'':userInfo.ID_type==1?'身份证': '护照'}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">证件号码</div>
					<div class="my-label">{{userInfo.ID_number}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">证件生效日期</div>
					<div class="my-label">{{userInfo.effective_start_date}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">证件有效期限</div>
					<div class="my-label">{{userInfo.effective_end_date}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">签发机关</div>
					<div class="my-label">{{userInfo.issuance_organ}}</div>
				</el-row>
				<el-row class="mb10">
					<div class="bold my-label label-width">证件证明</div>
					<div class="flex mb10">
						<el-image :preview-src-list="userInfo.imgArr" class="mr10" style="width: 100px; height: 100px"
							:src="imgs" fit="fit" v-for="(imgs,mIndex) in userInfo.imgArr" :key="mIndex"></el-image>
					</div>
				</el-row>
			</el-row> -->
		</template>
		<template v-if="pageType=='2'">
			<el-row class="">
				<div class="title-bg mb10">账号信息</div>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">机构名称</div>
					<div class="my-label">{{userInfo.username}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">注册日期</div>
					<div class="my-label">{{userInfo.createTime}}</div>
				</el-row>
				<el-row class="mb10 flex flex-center">
					<div class="bold my-label label-width">联系电话</div>
					<div class="my-label">{{userInfo.phonenumber}}</div>
				</el-row>
				
			</el-row>
		</template>
		<!-- <el-row class="mb20">
      <div class="title-bg mb10">其他信息</div>
      <el-row class="mb10 flex flex-center">
        <div class="bold my-label label-width">角色类型</div>
        <div class="my-label">{{userInfo.roleType}}</div>
      </el-row>
      <el-row class="mb10 flex flex-center">
        <div class="bold my-label label-width">是否是引导专员</div>
        <div class="my-label">{{userInfo.guideUserNum}}</div>
      </el-row>
      <el-row class="mb10 flex flex-center">
        <div class="bold my-label label-width">是否有引导专员</div>
        <div class="my-label">{{userInfo.guidedNum}}</div>
      </el-row>
    </el-row> -->
		<!-- <span slot="footer" class="dialog-footer">
      <el-button @click="delAutonym">删除认证信息</el-button>
    </span> -->
	</el-dialog>
</template>

<script>
	import {
		getUserCertification,
		getUserInfoOne
	} from "@/api/operationRoleApi/dispatchingManagementApi.js"
	import {
		getUserInfoByOrg
	} from '@/api/commonApi.js'
	export default {
		name: "index",
		props: {
			row: {
				type: Object,
				default: () => {
					return {}
				}
			},
			userId: {
				type: String,
				default: ''
			},
			pageType: {
				type: String,
				default: '1'
			}
		},
		data() {
			return {
				isOpen: true,
				fit: 'fit',
				imgUrl: this.$store.state.basics.img_url_user_idcard,
				userInfo: {
					username: '',
					userTag: '',
					phonenumber: '',
					createTime: '',
					guideUserNum: '',
					guidedNum: '',
					roleType: '',
					certification_id: '',
					user_id: '',
					realname: '',
					ID_type: '',
					ID_number: '',
					effective_start_date: '',
					effective_end_date: '',
					issuance_organ: '',
					create_time: '',
					imgArr: ''
				},
				title: '个人账号信息'
			};
		},
		methods: {
			close() {
				this.isOpen = false
				this.$emit('close')
			},
			beforeClose() {
				this.close()
			},
			// 获取实名认证信息
			async getUserCertification() {
				//this.row.user_id

				await getUserCertification({
					userId: this.row.userId || this.userId,
					curUserId: this.$store.state.user.adminId,
				}).then((res) => {
					if (res.Tag.length) {
						// this.userInfo = res.Tag[0].Table[0]
						let data = res.Tag[0].Table[0]
						this.userInfo.certification_id = data.certification_id
						this.userInfo.user_id = data.user_id
						this.userInfo.realname = data.realname
						this.userInfo.ID_type = data.ID_type
						this.userInfo.ID_number = data.ID_number
						this.userInfo.effective_start_date = data.effective_start_date
						this.userInfo.effective_end_date = data.effective_end_date
						this.userInfo.issuance_organ = data.issuance_organ
						this.userInfo.create_time = data.create_time
						this.userInfo.imgArr = data.imgs.split(',')

						for (let i in this.userInfo.imgArr) {
							this.userInfo.imgArr[i] = this.imgUrl + this.userInfo.imgArr[i]
						}
						console.log('实名认证', this.userInfo);
					}

				})
			},
			// 获取用户个人信息
			async getUserInfoOne() {
				await getUserInfoOne({
					userId: this.row.userId || this.userId,
					curUserId: this.$store.state.user.adminId,
				}).then((res) => {
					if (res.Tag.length) {
						let data = res.Tag[0].Table[0]
						this.userInfo.username = data.username || data.userName
						this.userInfo.userTag = data.userTag
						this.userInfo.nation = data.nation
						this.userInfo.phonenumber = data.phonenumber
						this.userInfo.createTime = data.createTime || data.registerTime
						this.userInfo.guideUserNum = data.guideUserNum > 0 ? '是' : '不是'
						this.userInfo.guidedNum = data.guidedNum > 0 ? '有' : '没有'
						console.log('个人信息111', this.userInfo);
					}
				})
			},
			// 获取机构用户信息
			async getUserInfoByOrg() {
				await getUserInfoByOrg({
					userId: this.row.userId || this.userId,
				}).then(res => {
					if(res.Tag.length) {
						let data = res.Tag[0].Table[0]
						this.userInfo.username = data.userName
						this.userInfo.phonenumber = data.phonenumber
						this.userInfo.createTime = data.createTime
					}
				})
			}
		},
		created() {
			if(this.pageType=='1') {
				// this.getUserCertification()
				this.getUserInfoOne()
				this.title = '个人账号信息'
			} else {
				this.getUserInfoByOrg()
				this.title = '机构账号信息'
			}
			console.log(this.row);
			
		}
	};
</script>

<style lang="scss" scoped>
	.title-bg {
		background-color: #F2F2F2;
		padding: 10px;
		font-weight: bold;
	}

	.my-label {
		padding-bottom: 8px;
	}

	.label-width {
		width: 120px;
	}
</style>
