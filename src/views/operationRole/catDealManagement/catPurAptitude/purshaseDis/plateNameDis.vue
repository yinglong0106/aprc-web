<template>
  <div class="app-container">
    <el-row>
      <span class="bold">品类名称：</span>
      <span class="ml10">{{openRow.categoryName}}</span>
    </el-row>
    <el-row class="mt20 mb20">
      <el-button @click="addPlate">添加板块</el-button>
    </el-row>
    <el-row>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="plateName" label="板块名称" align="center">
        </el-table-column>
        <el-table-column prop="plateAlias" label="板块别名" align="center">
        </el-table-column>
        <el-table-column label="操作" align="center">
          <template slot-scope="scope">
            <el-button @click="editNickname(scope.row,scope.$index)" type="text" size="small">编辑板块别名</el-button>
            <el-button @click="delNickname(scope.row,scope.$index)" type="text" size="small">删除板块名称</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-row>

    <el-dialog title="板块选择" width="700px" :visible.sync="isAdd" @close="closePlate" destroy-on-close>
    	<el-row>
    	  <span class="bold">品类名称：</span>
    	  <span class="ml10">{{openRow.categoryName}}</span>
    	</el-row>
    	<el-row class="mt20 pb20">
    		<div v-for="(item,index) in plate" :key="index">
    			<div class="flex jsb flex-center mb10" v-if="item.fixedDataName">
    				<div>{{item.fixedDataName}}</div>
    				<el-button v-if="item.check">已添加</el-button>
    				<el-button v-else type="primary" @click="checkPlate(index)">添加</el-button>
    			</div>
    		</div>
    	</el-row>
    </el-dialog>

    <el-dialog title="编辑板块别名" width="700px" :visible.sync="isEdit" destroy-on-close>
      <el-row>
        <div class="flex flex-center mt10">
          <div class="bold input-text">板块名称</div>
          <div class="">{{oldPlate.plateName}}</div>
        </div>
      </el-row>
      <el-row class="mt20 pb20">
        <div class="flex flex-center mb10">
          <div class="input-text">板块别名</div>
          <el-input placeholder="请输入" v-model="editName"></el-input>
        </div>
      </el-row>
      <span slot="footer" class="dialog-footer">
        <el-button @click="closeEdit">取 消</el-button>
        <el-button type="primary" @click="submitEdit" :disabled="!editName.trim()">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
	import {
		getPlates,
		getFreePlates,
		addPlate,
		getPlateAddFlag,
		updatePlateAlias,
		deletePlate
	} from '@/api/modelRoleApi/tradingContent.js'
	export default {
		name: "index",
		data() {
			return {
				tableData: [],
				isAdd: false,
				openRow: {},
				plate: [{
					name: '产品板块',
					check: false,
				}, {
					name: '数量板块',
					check: false,
				}, {
					name: '交接板块',
					check: false,
				}, {
					name: '材料板块',
					check: false,
				}],
				isEdit: false,
				oldPlate: {},
				editName: ''
			};
		},
		methods: {
			editNickname(row, index) {
				this.isEdit = true
				this.oldPlate = row
				this.editName = row.plateAlias
			},
			closeEdit() {
				this.isEdit = false
			},
			submitEdit() {
				console.log('提交');
				this.updatePlateAlias()

			},
			delNickname(row, index) {
				this.$confirm('请确认要删除' + row.plateName, '', {
					confirmButtonText: '确定',
					type: 'warning'
				}).then(() => {
					this.deletePlate(row.plateGuid)
					this.$message({
						type: 'success',
						message: '删除成功!'
					});
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});

			},
			addPlate(row) {
				this.isAdd = true
				this.getFreePlates()
			},
			closePlate() {
				this.isAdd = false
				this.getPlates()
			},
			checkPlate(index) {
				this.plate[index].check = true
				this.plate = this.clone(this.plate)
				// let plateId = this.plate[index].fixedDataCode
				// this.getPlateAddFlag(plateId)
				let plateObj = this.plate[index]
				this.addPlated(plateObj)

			},
			// 删除板块
			async deletePlate(id) {
				await deletePlate({
					plateGuid: id,
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					this.getPlates()
				})
			},
			// 编辑板块别名
			async updatePlateAlias() {
				await updatePlateAlias({
					plateGuid: this.oldPlate.plateGuid,
					plateAlias: this.editName.trim(),
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					console.log(res);
					this.closeEdit()
					this.getPlates()
					this.editName = ''
				})
			},
			// 添加板块 要先去请求固化库板块名称
			async addPlated(obj) {
				await addPlate({
					catTreeCode: this.openRow.type || this.openRow.catTreeCode,
					categoryGuid: this.openRow.categoryGuid,
					cattypeGuid: this.openRow.categoryGuid,
					bizType: this.openRow.bizType,
					curUserId: this.$store.state.user.adminId,
					fixedDataCode: obj.fixedDataCode,
					 fixedDataName: obj.fixedDataName,
				}).then(res => {
					if (res.Tag[0] > 0) {
						this.$message({
							message: '添加成功',
							type: 'success'
						})
					} else {
						this.$message({
							message: '添加失败',
							type: 'error'
						})
					}

				})
			},
			// 先用这个接口判断是否重复
			// async getPlateAddFlag(id) {
			// 	await getPlateAddFlag({
			// 		catTreeCode: this.openRow.type || this.openRow.catTreeCode,
			// 		categoryGuid: this.openRow.categoryGuid,
			// 		fixedDataGuid: id,
			// 		bizType: this.openRow.bizType,
			// 		curUserId: this.$store.state.user.adminId,
			// 	}).then(res => {
			// 		console.log(res);
			// 		if (res.Tag[0].Table[0].addFlag > 0) {
			// 			this.$message({
			// 				message: '已添加，不可以重复添加',
			// 				type: 'info'
			// 			})
			// 		} else {
			// 			this.addPlated(id)
			// 		}
			// 	})
			// },
			// 查询可添加的板块列表
			async getFreePlates() {
				await getFreePlates({
					catTreeCode: this.openRow.type || this.openRow.catTreeCode,
					categoryGuid: this.openRow.categoryGuid,
					bizType: this.openRow.bizType,
					fixedDataBizType: '4',
					page: 1,
					size: '200',
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					console.log(res);
					if (res.Tag.length) {
						let data = res.Tag[0].Table
						this.plate = data
					}
				})
			},
			async getPlates() {
				await getPlates({
					catTreeCode: this.openRow.type || this.openRow.catTreeCode,
					bizType: this.openRow.bizType,
					categoryGuid: this.openRow.categoryGuid,
					fixedDataBizType: '4',
					plateFixedDataCode: '',
					curUserId: this.$store.state.user.adminId,
				}).then(res => {
					console.log(res);
					if (res.Tag.length) {
						let data = res.Tag[0].Table
						this.tableData = data
					} else {
						this.tableData = []
					}
				})
			}
		},
		mounted() {
			this.openRow = this.$route.query
			this.getPlates()
		}
	};
</script>

<style lang="scss">
  .input-text {
    width: 100px;
    flex-shrink: 0;
  }
</style>
