<template>
  <div>
    <el-card v-bind:style="{ width: width + 'px' }" shadow="none">
      <div slot="header" class="clearfix">
        <span class="awardHeader">{{ level }}级奖励</span>
        <el-button
          style="float: right;"
          type="info"
          size="mini"
          @click="addAward"
        >
          添加奖励
        </el-button>
      </div>
      <div>
        <el-row :gutter="5">
          <el-col :span="12">
            <el-form-item label="原价" prop="oriPrice">
              <el-input v-model="item.oriPrice"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="奖品名称" prop="awardName">
              <el-input v-model="item.awardName"></el-input>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row :gutter="5">
          <el-col :span="12">
            <el-form-item label="需邀请人数" prop="inviteNum">
              <el-select v-model="item.inviteNum" placeholder="选择邀请人数">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="虚拟商品" prop="virtualItem">
              <el-checkbox v-model="item.virtualItem"></el-checkbox>
            </el-form-item>
          </el-col>
        </el-row>

        <el-form-item label="奖品图片配置" prop="awardPicUrl">
          <el-upload
            :action="actionUrl"
            list-type="picture-card"
            :file-list="item.awardPicUrl"
            :on-remove="handleRemove"
            :on-success="handleSuccess"
            :before-upload="beforeUpload"
            :limit="1"
            accept=".jpg,.png"
          >
            <i class="el-icon-plus"></i>
          </el-upload>
          <span>
            (格式：支持一张图，jpg/png, 大小不超过400k,尺寸限制：'750*560')
          </span>
        </el-form-item>
      </div>
      <div class="bottom clearfix">
        <el-button
          type="text"
          class="button"
          style="float:right;"
          v-if="one == 1 ? false : true"
          @click="delAward"
        >
          删除
        </el-button>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'AwardConfig',
  data() {
    return {
      options: []
    }
  },
  props: {
    acId: { type: Number, required: true },
    width: { type: Number, required: false, default: 500 },
    level: { type: String, required: true },
    one: { type: Number, required: true, default: 1 },
    item: { type: Object, required: true },
    actionUrl: { type: String, required: true }
  },
  methods: {
    beforeUpload(f) {
      let res = true
      this.$emit('beforeUpload', f, val => {
        res = val
      })
      return res
    },
    handleRemove(f) {
      this.$emit('handleRemove', f)
    },
    handleSuccess(response, file, fileList) {
      this.$emit('handleSuccess', response, file, fileList)
    },
    addAward() {
      this.$emit('addAward')
    },
    delAward() {
      this.$emit('delAward')
    },
    createOptions() {
      let i = 1
      for (let i = 1; i <= 50; i++) {
        this.options.push({
          value: i,
          label: i
        })
      }
    }
  },
  created() {
    this.createOptions()
  }
}
</script>
<style scoped>
.el-form--label-left .el-form-item__label {
  text-align: right;
}
.el-card {
  margin-bottom: 10px;
}
.awardHeader {
  line-height: 28px;
  font-size: 23px;
}
.clearfix:before,
.clearfix:after {
  display: table;
  content: '';
}

.clearfix:after {
  clear: both;
}
</style>
