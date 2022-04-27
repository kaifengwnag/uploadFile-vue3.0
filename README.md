# uploadFile-vue3.0
vue3.0 单文件上传 ，antd-Vue 按钮和 message组件，input[type="file"] H5标签

## 使用说明
  # 可在全局中（main.ts）注册 组件
    import UploadFile from "./components/uploadFile/index.vue"
    app.component('UploadFile', UploadFile)
  # 父组件
    <UploadFile @successChange="successChange"/>
    /*
    * 上传成功的回调
    * value { String } 路径
    *
    */
    const successChange = (value) => {
      console.log(value)
    }
