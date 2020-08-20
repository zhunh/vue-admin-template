<template>
  <div style="padding:30px;">
    <el-form>
      <award-config
        v-for="(item, index) in awardArr"
        :acId="index"
        :key="index"
        :one="awardArr.length"
        :width="800"
        :item="item"
        :actionUrl="picUrl"
        :level="awardLevel[index]"
        v-on:addAward="addAwardB"
        v-on:delAward="delAwardB(index)"
        v-on:handleSuccess="uploadAwardPicSuc"
        :handleRemove="(file, fileList) => {}"
        v-on:beforeUpload="beforeUploadAwardPic"
      />
    </el-form>
  </div>
</template>
<script>
import awardConfig from "./components/awardConfig";
export default {
  components: { awardConfig },
  data() {
    return {
      picUrl: "ui",
      awardLevel: ["一", "二", "三"],
      awardPicList: [],
      awardArr: [
        {
          oriPrice: "", //原价
          awardName: "", //奖品名称
          inviteNum: "", //需邀请人数
          virtualItem: false, //是否为虚拟商品
          awardPicUrl: [] //奖品图片列表
        }
      ] //奖品配置框组
    };
  },
  methods: {
    // 添加奖励
    addAwardB() {
      console.log(this.awardArr);
      if (this.awardArr.length >= 3) {
        this.$message({
          message: "最多添加三个奖励",
          type: "warning"
        });
        return;
      }
      this.awardArr.push({
        oriPrice: "",
        awardName: "",
        inviteNum: "",
        virtualItem: false,
        awardPicUrl: []
      });
    },
    // 删除奖励
    delAwardB(index) {
      this.awardArr.splice(index, 1);
    },
    //上传奖品图片成功的钩子函数
    uploadAwardPicSuc(res, file, fileList) {
      console.log(res);
      console.log(file);
      this.awardPicList.push({
        uid: file.uid,
        status: file.status,
        url: res.data[0]
      });
    },
    // 上传奖品图片大小校验
    beforeUploadAwardPic(f, callback) {
      if (f.size > 400 * 1024) {
        this.$message({message:"图片大小不可以超过400k！",type:"warning"});
        return callback(false);
      }
      return callback(true);
    }
  }
};
</script>
