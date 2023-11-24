<template>
  <div class="dashboard-container">
    <div class="parameter-settings">
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="网络模型">
          <el-select v-model="form.networkModel" placeholder="请选择">
            <el-option label="WS小世界超网络" value="ws" @click.native="selectWS" />
            <el-option label="NW小世界超网络" value="nw" @click.native="selectNW" />
          </el-select>
          <div>
            <el-dialog
              title="提示"
              :visible.sync="wsDialogVisible"
              width="25%"
            >
              <el-form :model="wsForm" label-width="120px">
                <el-form-item label="节点总数N">
                  <el-input v-model="wsForm.N" placeholder="请输入内容" />
                </el-form-item>
                <el-form-item label="相邻节点数K">
                  <el-input v-model="wsForm.K" placeholder="请输入内容" />
                </el-form-item>
                <el-form-item label="概率p">
                  <el-input v-model="wsForm.p" placeholder="请输入内容" />
                </el-form-item>
              </el-form>
              <span slot="footer" class="dialog-footer">
                <el-button @click="wsDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="wsSubmit">确 定</el-button>
              </span>
            </el-dialog>
            <el-dialog
              title="提示"
              :visible.sync="nwDialogVisible"
              width="25%"
            >
              <el-form :model="nwForm" label-width="120px">
                <el-form-item label="节点总数N">
                  <el-input v-model="nwForm.N" placeholder="请输入内容" />
                </el-form-item>
                <el-form-item label="相邻节点数K">
                  <el-input v-model="nwForm.K" placeholder="请输入内容" />
                </el-form-item>
                <el-form-item label="概率p">
                  <el-input v-model="nwForm.p" placeholder="请输入内容" />
                </el-form-item>
              </el-form>
              <span slot="footer" class="dialog-footer">
                <el-button @click="nwDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="nwDialogVisible = false">确 定</el-button>
              </span>
            </el-dialog>
          </div>
        </el-form-item>
        <el-form-item label="传播模型">
          <el-select v-model="form.spreadingModel" placeholder="请选择">
            <el-option label="SIS" value="sis" />
            <el-option label="SIR" value="sir" />
          </el-select>
        </el-form-item>
        <el-form-item label="传播策略">
          <el-select v-model="form.spreadingStrategy" placeholder="请选择">
            <el-option label="RP" value="rp" />
            <el-option label="CP" value="cp" />
          </el-select>
        </el-form-item>
        <el-form-item label="感染率">
          <el-input v-model="form.infectionRate" placeholder="请输入内容" />
        </el-form-item>
        <el-form-item label="恢复率">
          <el-input v-model="form.recoveryRate" placeholder="请输入内容" />
        </el-form-item>
        <el-form-item label="移除率">
          <el-input v-model="form.removalRate" placeholder="请输入内容" />
        </el-form-item>
        <el-form-item label="传播时间步">
          <el-input v-model="form.timeStep" placeholder="请输入内容" />
        </el-form-item>
        <el-form-item label="初始感染节点">
          <el-input v-model="form.initialNode" placeholder="请输入内容" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">Create</el-button>
          <el-button @click="onCancel">Cancel</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="spreading-img" />
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'

export default {
  data() {
    return {
      wsDialogVisible: false,
      nwDialogVisible: false,
      wsForm: {
        N: '',
        K: '',
        p: ''
      },
      nwForm: {
        N: '',
        K: '',
        p: ''
      },
      form: {
        networkModel: '',
        spreadingModel: '',
        spreadingStrategy: '',
        infectionRate: '',
        recoveryRate: '',
        removalRate: '',
        timeStep: '',
        initialNode: ''
      }
    }
  },
  methods: {
    selectWS() {
      this.wsDialogVisible = true
    },
    selectNW() {
      this.nwDialogVisible = true
    },
    wsSubmit() {
      console.log(this.wsForm)
      this.axios(
        {
          url: '/hypernetwork/ws',
          method: 'POST',
          data: this.wsForm
        }
      ).then(response => {
        console.log(response)
      })
      this.wsDialogVisible = false
    },
    onSubmit() {
      this.$message('submit!')
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
.line{
  text-align: center;
}
.el-form-item .el-input{
  width: 300px;
}

.el-form-item .el-select{
  width: 300px;
}
.spreading-img {
  width: 900px;
  height: 700px;
  background-color: gray;
  margin-left: 50px;
}
.dashboard-container{
  display: flex;
}
.el-dialog .el-form .el-form-item{
  margin-bottom: 20px;
}
</style>
