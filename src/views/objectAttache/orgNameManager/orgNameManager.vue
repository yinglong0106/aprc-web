<template>
	<div class="app-container">
		<div class="flex jsb flex-center">
			<searchCom @toSearch='search' :searchResult='searchResult' placeholderText='请输入你要找的机构名称'></searchCom>
			<div>
				<el-button type="primary" @click="openNew">创建供应机构账号</el-button>
			</div>
		</div>
		<el-table :data="tableData" border v-loading="loading">
			<el-table-column prop="orgName" label="机构名称" align="center"></el-table-column>
			<el-table-column prop="createTime" label="创建日期" align="center" width="240"></el-table-column>
			<el-table-column label="操作" align="center" width="640">
				<template slot-scope="scope">
					<el-button type="text" @click="editOrg(scope.row)">编辑机构名称</el-button>
					<el-button type="text" @click="delOrg(scope.row)">删除机构名称</el-button>
					<!-- <el-button type="text" @click="toRegisterVerify(scope.row)">注册验证号码管理</el-button> -->
					<el-button type="text" @click="toLoginPhoneManage(scope.row)">登录手机号管理</el-button>
					<!-- <el-button type="text" @click="openRelevanceSupplyAttach(scope.row)">关联供应专员管理</el-button> -->
					<el-button type="text" @click="openJurisdictionManage(scope.row)">系统登录权限管理</el-button>
					<el-button type="text" @click="openRelevanceSupplyAttach(scope.row)">账号申请人管理</el-button>
				</template>
			</el-table-column>
		</el-table>
		<pages @changePage="changePage" :total="pageTotal" :page="page"></pages>
		<newOrganization v-if="isNew" @close="closeNew" @refresh="getOrgList" :row="openRow" :pageType="pageType">
		</newOrganization>
		<loginPhoneManage v-if="isPhone" @close="closeLoginPhoneManage" @refresh="getOrgList" :row="openRow">
		</loginPhoneManage>
		<relevanceSupplyAttach v-if="isRelevance" @close="closeRelevance" @refresh="getOrgList" :row="openRow"></relevanceSupplyAttach>
		<jurisdictionManage v-if="isJurisdiction" @close="closeJurisdictionManage" @refresh="getOrgList" :row="openRow"></jurisdictionManage>
	</div>
</template>

<script>
	import searchCom from '@/views/components/common/searchCom.vue'
	import pages from '@/views/components/common/pages'
	import newOrganization from '@/views/objectAttache/orgNameManager/components/newOrg.vue'
	import loginPhoneManage from '@/views/objectAttache/orgNameManager/components/loginPhoneManage.vue'
	import relevanceSupplyAttach from '@/views/objectAttache/orgNameManager/components/relevanceSupplyAttach.vue'
	import jurisdictionManage from '@/views/objectAttache/orgNameManager/components/jurisdictionManage'
	import {
		getOrgList,
		deleteOrg
	} from '@/api/objectAttacheApi/objectAttache.js'
	export default {
		name: "index",
		components: {
			searchCom,
			pages,
			newOrganization,
			loginPhoneManage,
			relevanceSupplyAttach,
			jurisdictionManage
		},
		data() {
			return {
				loading: false,
				tableData: [],
				page: 1,
				pageTotal: 0,
				searchResult: 0,
				searchVal: '',
				isNew: false,
				openRow: {},
				pageType: 'new',
				isPhone: false,
				isRelevance: false,
				isJurisdiction: false,
			};
		},
		methods: {
			search(data) {
				this.searchVal = data
				this.page = 1
				this.getOrgList()
			},
			changePage(page) {
				this.page = page
				this.getOrgList()
			},
			openNew() {
				this.isNew = true
				this.openRow = {}
				this.pageType = 'new'
			},
			closeNew() {
				this.isNew = false
			},
			editOrg(row) {
				console.log(row);
				this.openRow = row
				this.isNew = true
				this.pageType = 'edit'
			},
			delOrg(row) {
				// let msg = '【' + row.orgName + '】删除后，全网不可见这个机构的有关信息。确认删除？'
				// this.$confirm(msg, '', {
				// 	confirmButtonText: '确定',
				// 	cancelButtonText: '取消',
				// 	type: 'warning'
				// }).then(() => {

				// 	this.deleteOrg(row)
				// }).catch(() => {
				// 	this.$message({
				// 		type: 'info',
				// 		message: '已取消删除'
				// 	});
				// });

				this.deleteOrg(row)

			},
			toLoginPhoneManage(row) {
				this.openRow = row
				this.isPhone = true
			},
			closeLoginPhoneManage() {
				this.isPhone = false
			},
			openRelevanceSupplyAttach(row) {
				this.openRow = row
				this.isRelevance = true
			},
			closeRelevance() {
				this.isRelevance = false
			},
			openJurisdictionManage(row) {
				this.isJurisdiction = true
				this.openRow = row
			},
			closeJurisdictionManage() {
				this.isJurisdiction = false
			},
			signLog(row) {
				this.openRow = row
			},
			async deleteOrg(row) {
				await deleteOrg({
					orgGuid: row.orgGuid,
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					if (res.OK == 'True') {

						if (res.Tag[0].Table[0].delFalg > 0) {
							let msg = res.Tag[0].Table[0].notOkReason
							this.$confirm(msg, '提示', {
								confirmButtonText: '我知道了',
								dangerouslyUseHTMLString: true,
								showCancelButton: false,
								type: 'warning'
							}).then(() => {
								this.getOrgList()
							}).catch(() => {});

						} else {
							let msg = res.Tag[0].Table[0].notOkReason
							// 不可以发布

							this.$confirm(msg, '提示', {
								confirmButtonText: '我知道了',
								dangerouslyUseHTMLString: true,
								showCancelButton: false,
								type: 'warning'
							}).then(() => {
								this.getOrgList()
							}).catch(() => {});

						}

					}
				})
			},
			async getOrgList() {
				this.loading = true
				await getOrgList({
					orgName: this.searchVal,
					size: '20',
					page: this.page,
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					this.loading = false
					if (res.OK == 'True') {

						console.log(res);
						if (res.Tag.length > 0) {
							let data = res.Tag[0].Table
							this.tableData = data
						} else {
							this.tableData = []
						}
						this.searchResult = this.tableData.length
						this.pageTotal = this.tableData.length > 0 ? (this.page - 1) * 20 + 21 : (this.page -
							1) * 20 + 1
					}
				})
			},
		},
		created() {
			this.getOrgList()
		}
	}
</script>

<style scoped lang="scss">
</style>
