<template>
  <div>
    <!-- Create toolbar container -->
    <div id="toolbar" v-if="!this.disabled"></div>
    <div id="toolbar" v-if="this.disabled">
      <el-row :gutter="2" style="margin: 0 auto">
        <el-col :span="4"
          ><div>
            <button class="ql-bold"></button>
            <button class="ql-italic"></button>
            <button class="ql-underline"></button>
            <button class="ql-strike"></button></div
        ></el-col>
        <el-col :span="2"
          ><div>
            <button class="ql-blockquote"></button>
            <button class="ql-code-block"></button></div
        ></el-col>
        <el-col :span="2"
          ><div>
            <button class="ql-list" value="ordered"></button>
            <button class="ql-list" value="bullet"></button></div
        ></el-col>
        <el-col :span="2"
          ><div>
            <button class="ql-script" value="sub"></button>
            <button class="ql-script" value="super"></button></div
        ></el-col>
        <el-col :span="2"
          ><div>
            <button class="ql-indent" value="-1"></button>
            <button class="ql-indent" value="+1"></button></div
        ></el-col>
        <el-col :span="4"
          ><div>
            <select class="ql-header">
              <option value="1"></option>
              <option value="2"></option>
              <option value="3"></option>
              <option value="4"></option>
              <option value="5"></option>
              <option value="6"></option>
              <option value="false" selected></option>
            </select></div
        ></el-col>
        <el-col :span="3"
          ><div>
            <select class="ql-color"></select>
            <select class="ql-background"></select></div
        ></el-col>
        <el-col :span="2"
          ><div>
            <select class="ql-align"></select></div
        ></el-col>
        <el-col :span="3"
          ><div>
            <button class="ql-link"></button>
            <button class="ql-image"></button>
            <button class="ql-video"></button></div
        ></el-col>
      </el-row>
      <!-- Add font size dropdown -->
    </div>
    <el-scrollbar :style="{ height: eHeight }">
      <div id="editor"></div>
    </el-scrollbar>
  </div>
</template>

<script>
import Quill from "quill";
import "quill/dist/quill.snow.css";
export default {
  name: "QuillEditor",
  props: {
    value: Object,
    eHeight: String,
    //disabled: true,
  },
  data() {
    return {
      quill: null,
    };
  },
  mounted() {
    // let dom = this.$el.querySelector('.editor')
    this.quill = new Quill("#editor", {
      theme: "snow",
      modules: {
        toolbar: "#toolbar",
      },
      placeholder: "Insert text here ...",
    });
    this.quill.setContents(this.value);
    this.quill.on("text-change", () => {
      this.$emit("input", this.quill.getContents());
    });
    this.quill.enable(this.disabled);
  },
  watch: {},
  methods: {
    setContents(val) {
      console.log("set contents");
      this.quill.setContents(val);
    },
  },
};
</script>

<style scoped>
.ql-container {
  height: 1200px;
  width: 800px;
  margin: 0 auto;
}
#editor {
  border: 1px solid #dcdfe6;
  border-radius: 4px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  margin-top: 5px;
  margin-bottom: 20px;
}
.ql-toolbar {
  border: 0px;
}
#toolbar {
  width: 800px;
  margin: 0 auto;
}

.el-scrollbar__wrap {
  overflow-x: hidden;
}
.el-scrollbar__bar.is-horizontal {
  display: none;
}
</style>
