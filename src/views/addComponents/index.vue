<template>
  <div style="padding:30px;">
    <p>动态添加组件，字段校验绑定。</p>
    <el-form
      ref="ruleForm"
      :model="ruleForm"
      label-width="150px"
      label-position="right"    
    >
      <el-card
        shadow="none"
        v-for="(item, index) in ruleForm.awardConfigList"
        :key="index"
        style="width:800px;"
      >
        <div slot="header" class="clearfix">
          <span class="awardHeader">{{ awardLevel[index] }}级奖励</span>
          <el-button
            style="float: right;"
            type="primary"
            size="mini"
            @click="addAward"
          >
            添加奖励
          </el-button>
        </div>
        <div>
          <el-row :gutter="5">
            <el-col :span="12">
              <el-form-item label="原价" 
                :prop="`awardConfigList[${index}].oriPrice`"
                :rules="{ required: true, message: '请输入原价',trigger:'blur' }"
                >
                <el-input v-model="item.oriPrice"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item
                label="奖品名称"
                :prop="`awardConfigList[${index}].awardName`"
                :rules="{ required: true, message: '请输入奖品名称',trigger:'blur' }"
              >
                <el-input v-model="item.awardName"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="12">
              <el-form-item
                label="需邀请人数"
                :prop="`awardConfigList[${index}].inviteNum`"
                :rules="{ required: true,message: '需邀请人数不可以为空',trigger:'blur' }"
              >
                <el-select v-model="item.inviteNum" placeholder="选择邀请人数">
                  <el-option
                    v-for="index in 50"
                    :key="index"
                    :label="index+1"
                    :value="index+1"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="3">
              <el-form-item label="虚拟商品" prop="isVirtualItem">
                <el-checkbox v-model="item.isVirtualItem"></el-checkbox>
              </el-form-item>
            </el-col>
            <el-col :span="9">
              <el-form-item v-if="item.isVirtualItem" label="商品ID"
               :prop="`awardConfigList[${index}].vItemId`"
               :rules="{ required: true,message: '请填写商品ID',trigger:'blur' }"
               >
                <el-input v-model="item.vItemId"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <el-form-item
            label="奖品图片配置"
            :prop="`awardConfigList[${index}].awardPicUrl`"
            :rules="{ required: true, message: '请上传奖品图片' }"
          >
            <el-upload
              :action="picUrl"
              list-type="picture-card"
              :file-list="item.awardPicUrl"
              :on-remove="()=>{}"
              :on-success="
                (res, file, fileList) => {
                  return uploadAwardPicSuc(res, file, fileList, index)
                }         
              "
              :before-upload="beforeUploadAwardPic"
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
            v-if="ruleForm.awardConfigList.length == 1 ? false : true"
            @click="delAward"
          >
            删除
          </el-button>
        </div>
      </el-card>
    </el-form>
  </div>
</template>
<script>
export default {
  data(){
    return{
      picUrl: '',
      awardLevel: ['一', '二', '三'],
      ruleForm: {
        options:[],
        awardConfigList: [ //配置奖品列表
          {
            oriPrice: '',
            awardName: '',
            inviteNum: '',
            isVirtualItem: '',
            vItemId: '',
            awardPicUrl: []
          }
        ]
      },
    }
  },
  methods:{
    addAward() {
      if (this.ruleForm.awardConfigList.length >= 3) {
        this.$message({
          message: '最多添加三个奖励',
          type: 'warning'
        })
        return
      }      
      this.ruleForm.awardConfigList.push({
        oriPrice: '',
        awardName: '',
        inviteNum: '',
        isVirtualItem: '',
        vItemId: '',
        awardPicUrl: []
      })
    },
    delAward(index) {
      this.ruleForm.awardConfigList.splice(index, 1)
    },
    // 上传奖品图片大小校验
    beforeUploadAwardPic(f){
      if(f.size > 400 * 1024){
        this.warning("图片大小不可以超过400k！")
        return false
      }
      return true
    }    
  }
}
</script>
<style scoped>
.el-card {
  margin-bottom: 10px;
}
.awardHeader {
  line-height: 28px;
  font-size: 23px;
}
</style>