<template>
  <div id="app">
    <el-container>
      <el-header>新旧编辑器内容转换</el-header>
      <el-main>
        <el-row>
          <el-input
            type="textarea"
            :rows="20"
            placeholder="请输入旧编辑器源码"
            v-model="code"
          >
          </el-input>
        </el-row>
        <el-row>
          <el-button type="primary" @click="transform">转换</el-button>
        </el-row>
      </el-main>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      code: "",
    };
  },
  methods: {
    transform() {
      let code = this.code;
      if (/^TFL/.test(code)) {
        this.$message.error("请不要重复转换");
        return;
      }
      code = code.replace(/<p>\s*<\/p>/ig,'');
      code = code.replace(
        /x-small|small|medium|large|x-large|xx-large/g,
        function(size) {
          switch (size) {
            case "x-small":
              return "12px";
            case "small":
              return "14px";
            case "medium":
              return "16px";
            case "large":
              return "18px";
            case "x-large":
              return "24px";
            case "xx-large":
              return "32px";
          }
        }
      );
      code = code.replace(/\n/g, "");
      code = code.replace(/'/g, "\\'");
      this.code = `TFL.cherryRichtext.activeEditor.setContent('${code}');`;
      this.$message({
        showClose: true,
        message: "转换成功，请复制到调试窗口执行",
        type: "success",
      });
    },
  },
};
</script>

<style>
.el-header {
  text-align: center;
  line-height: 60px;
  font-size: 32px;
}
.el-main {
  text-align: center;
}
.el-textarea {
  width: 80% !important;
}
.el-row {
  margin-bottom: 20px;
}
</style>
