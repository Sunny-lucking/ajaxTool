<template>
  <div id="app">
    <el-form ref="form" label-width="80px" >
      <el-tabs v-model="activeName" @tab-click="handleClick" type="card" style="margin-left: 20px;">
        <el-tab-pane label="请求修改" name="first">
          <el-collapse v-model="reqActiveNames" @change="handleChange">
            <el-collapse-item :name="item.id" v-for="(item) in reqList" :key="item.id" :title="item.tag">
              <el-form-item>
                <el-row :gutter="20">
                  <el-col :span="4">
                    <el-input v-model="item.tag" placeholder="tag" @change="onChangeReqData"></el-input>
                  </el-col>
                  <el-col :span="8">
                    <el-input v-model="item.url" placeholder="请输入需要修改的url" @change="onChangeReqData"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <el-select v-model="item.method" placeholder="请求方式" @change="onChangeReqData">
                      <el-option label="GET" value="GET"></el-option>
                      <el-option label="POST" value="POST"></el-option>
                    </el-select>
                  </el-col>
                  <el-col :span="4">
                    <el-switch v-model="item.swicthOn" @change="onChangeReqData"></el-switch>
                  </el-col>
                  <el-col :span="4">
                    <div class="el-icon-delete app-theme" @click="deleteItem(item.id)"></div>
                  </el-col>
                </el-row>
                <el-col :span="16" style="margin-top: 10px;">
                  <el-col :span="8">
                    <el-input v-model="item.url" placeholder="修改成为url" @change="onChangeReqData"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <el-select v-model="item.method" placeholder="修改成请求方式" @change="onChangeReqData">
                      <el-option label="GET" value="GET"></el-option>
                      <el-option label="POST" value="POST"></el-option>
                    </el-select>
                  </el-col>
                </el-col>
              </el-form-item>
            </el-collapse-item>
          </el-collapse>
        </el-tab-pane>
        <el-tab-pane label="响应修改" name="second">
          <el-collapse v-model="resActiveNames" @change="handleChange">
            <el-collapse-item :name="item.id" v-for="(item) in resList" :key="item.id" :title="item.tag">
              <el-form-item>
                <el-row :gutter="20">
                  <el-col :span="4">
                    <el-input v-model="item.tag" placeholder="tag" @change="onChangeData"></el-input>
                  </el-col>
                  <el-col :span="8">
                    <el-input v-model="item.url" placeholder="请输入url" @change="onChangeData"></el-input>
                  </el-col>
                  <el-col :span="4">
                    <el-select v-model="item.method" placeholder="请求方式" @change="onChangeData">
                      <el-option label="GET" value="GET"></el-option>
                      <el-option label="POST" value="POST"></el-option>
                    </el-select>
                  </el-col>
                  <el-col :span="4">
                    <el-switch v-model="item.swicthOn" @change="onChangeData"></el-switch>
                  </el-col>
                  <el-col :span="4">
                    <div class="el-icon-delete app-theme" @click="deleteItem(item.id)"></div>
                  </el-col>
                </el-row>
                <!-- <el-col :span="16" style="margin-top: 10px;">
                  <el-input type="textarea" v-model="item.response" @change="onChangeData"></el-input>
                </el-col> -->
                <vue-json-editor v-model="item.response" :showBtns="false" :mode="'code'" @json-change="onJsonChange"
                  @json-save="onJsonSave" @has-error="onError" style="text-align: left; resize: both;"/>
              </el-form-item>
            </el-collapse-item>
          </el-collapse>
        </el-tab-pane>
        <div class="el-icon-circle-plus-outline app-theme" @click="add"></div>
      </el-tabs>
    </el-form>
  </div>
</template>

<script>
import { nanoid } from 'nanoid'
import { Button, Form, Radio, FormItem, Col, Row, Input, Select, Switch, Option, Tabs, TabPane, Collapse, CollapseItem } from "element-ui";
import vueJsonEditor from 'vue-json-editor'
import Vue from 'vue';
Vue.use(Button)
Vue.use(Form)
Vue.use(Radio)
Vue.use(FormItem)
Vue.use(Col)
Vue.use(Input)
Vue.use(Select)
Vue.use(Switch)
Vue.use(Row)
Vue.use(Option)
Vue.use(Tabs)
Vue.use(TabPane)
Vue.use(Collapse)
Vue.use(CollapseItem)
export default {
  name: 'App',
  components: {
    vueJsonEditor
  },
  data() {
    return {
      resActiveNames: [],
      reqActiveNames: [],
      activeName: 'first',
      resList: [],
      reqList: []
    }
  },
  methods: {
    add() {
      let obj;
      const id = nanoid(5);
      if (this.activeName === 'first') {
        this.reqActiveNames.push(id);
        obj = {
          id,
          tag: id,
          url: '',
          method: 'GET',
          swicthOn: true,
          response: {}
        }
        this.reqList.push(obj)
      } else {
        this.resActiveNames.push(id)
        obj = {
          id,
          tag: id,
          url: '',
          method: 'GET',
          swicthOn: true,
          response: ''
        }
        this.resList.push(obj)
      }
    },
    deleteItem(id) {
      if (this.activeName === 'first') {
        this.reqList = this.resList.filter(item => item.id !== id)
        this.reqActiveNames = this.reqActiveNames.filter(item => item !== id)
      } else {
        this.resList = this.resList.filter(item => item.id !== id)
        this.resActiveNames = this.resActiveNames.filter(item => item !== id)
      }

    },
    handleChange() {

    },
    handleClick() {

    },
    onChangeReqData() {
      console.log(this.reqList);
    },
    onChangeData() {
      console.log(this.resList);
    },
    onJsonChange (value) {
        // console.log('更改value:', value);
        // 实时保存
        this.onJsonSave(value)
      },
      onJsonSave (value) {
        // console.log('保存value:', value);
        this.resultInfo = value
        this.hasJsonFlag = true
      },
      onError() {
        // console.log("json错误了value:", value);
        this.hasJsonFlag = false
      },
      // 检查json
      checkJson(){
        if (this.hasJsonFlag == false){
          // console.log("json验证失败")
          // this.$message.error("json验证失败")
          alert("json验证失败")
          return false
        } else {
          // console.log("json验证成功")
          // this.$message.success("json验证成功")
          alert("json验证成功")
          return true
        }
      },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.app-theme {
  font-size: 36px;
  color: #409EFF;
}
.jsoneditor-mode-code {
  resize: both;
}
</style>
