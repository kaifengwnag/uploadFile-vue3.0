<template>
  <input
    type="file"
    name="上传附件"
    id="file"
    multiple="false"
    ref="file"
    accept=".doc,.docx,.xlsx,.image,.png"
    @change="handleChange"
  />
  <a-button :loading="loading" @click="startsUpload">
    <upload-outlined></upload-outlined>
    上传文件
  </a-button>
</template>

<script>
import { defineComponent, ref } from "vue";
import { request } from "/src/api/service";
import { message } from "ant-design-vue";
import { useUserStore } from "/src/store/modules/user";

export default defineComponent({
  emits: ["successChange", "errorChange"],
  setup(proxy, { emit }) {
    const file = ref(null);
    const loading = ref(false);
    const userStore = useUserStore();
    const token = userStore.getToken;
    function handleChange(event) {
      let e = window.event || event;
      let formData = new FormData();
      if (e.target.files && e.target.files.length > 0) {
        loading.value = true;
        formData.append("file", e.target.files[0]);
        request({
          url: "/api/file-management/files/upload",
          method: "post",
          headers: {
            Authorization: "Bearer " + token
          },
          data: formData
        }).then((res) => {
          if(typeof res === "string") {
            emit("successChange", res);
            message.success(e.target.files.length + "个文件上传成功！！");
          } else {
            emit("errorChange", res);
            message.error(e.target.files.length + "个文件上传失败！！");
          }
          e.target.value = "";
          loading.value = false;
        }).catch(() => {
          e.target.value = "";
          loading.value = false;
        })
      }
    }
    function startsUpload() {
      file.value.click();
    }
    return {
      file,
      loading,
      startsUpload,
      handleChange
    };
  }
});
</script>

<style>
#file {
  display: none;
}
</style>
