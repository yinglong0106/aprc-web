<template>
	<el-dialog title="供应操作权限管理" :visible.sync="isOpen" width="700px" @close="beforeClose">
		<el-row class="mb20">
			<el-table :data="tableData" border>
			  <el-table-column prop="userName" label="姓名" align="center">
			  </el-table-column>
			  <el-table-column prop="nation" label="区号" align="center">
			  </el-table-column>
			  <el-table-column prop="phonenumber" label="联系电话" align="center">
			  </el-table-column>
			</el-table>
		</el-row>
		<el-row class="flex flex-center" style="padding: 10px 20px 20px 20px;">
			<div class="bold mr10">供应操作使用</div>
			<div>
				<el-switch v-model="isUse" active-color="#13ce66" inactive-color="#ccc" @change="changeUse">
				</el-switch>
			</div>
		</el-row>
	</el-dialog>
</template>

<script>
	import {
		permissionGetOneByType123,
		permissionUpdateByType3
	} from "@/api/operationRoleApi/dispatchingManagementApi.js"
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
				isUse: true,
				tableData: [],
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
			refresh() {
				this.$emit('refresh')
			},
			changeUse(val) {
				console.log('isUse', val);
				console.log('row', this.row);
				const msg = `(` + this.row.nation + `)` + this.row.phonenumber
				if (!val) {

					const h = this.$createElement;
					this.$msgbox({
						title: '',
						message: h('p', null, [
							h('span', null, '确认收回账号' + msg + '供应权限 '),
							h('h4', {
								style: 'font-size: 20px'
							}, '特别提示！！！'),
							h('div', null, '未添加的不能添加；已添加的自动删除。')
						]),
						confirmButtonText: '确定',
					}).then(action => {
						// this.isUse = false
						this.permissionUpdateByType3()
					});
				} else {
					// this.isUse = true
					this.permissionUpdateByType3()
				}
			},
			async permissionGetOneByType123() {
				await permissionGetOneByType123({
					userId: this.row.userId,
					type: 3,
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					console.log(res);
					if(res.Tag.length) {
						let data = res.Tag[0].Table
						this.isUse = data[0].status == 1?false:true
					}
				})
			},
			async permissionUpdateByType3() {
				await permissionUpdateByType3({
					userId: this.row.userId,
					status: this.isUse == true?0:1,
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					this.refresh()
					if (res.Tag[0]>0) {
					  this.$message({
					    type: 'success',
					    message: '操作成功!'
					  });
					} else {
					  this.$message({
					    type: 'error',
					    message: '操作失败!'
					  });
					}
				})
			}
		},
		created() {
			this.tableData.push(this.row)
			this.permissionGetOneByType123()
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
</style>


