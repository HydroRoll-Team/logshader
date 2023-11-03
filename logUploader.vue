<template>
    <v-app>
      <v-container>
        <v-file-input label="选择日志文件" v-model="selectedFile"></v-file-input>
  
        <v-btn color="primary" @click="uploadFile">上传</v-btn>
  
        <v-card>
          <v-card-text>
            <pre v-html="formattedLog"></pre>
          </v-card-text>
        </v-card>
      </v-container>
    </v-app>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedFile: null,
        logContent: '',
      }
    },
  
    computed: {
      formattedLog() {
        return this.logContent.replace(/\[(.*?)\]/g, '<span class="log-tag">$1</span>');
      },
    },
  
    methods: {
      async uploadFile() {
        if (!this.selectedFile) {
          alert('请先选择日志文件！');
          return;
        }
  
        const formData = new FormData();
        formData.append('file', this.selectedFile);
  
        try {
          const response = await fetch('/api/upload', { method: 'POST', body: formData });
          const data = await response.json();
  
          if (data.success) {
            this.logContent = data.logContent;
          } else {
            alert('上传出错！');
          }
        } catch (error) {
          console.error(error);
          alert('发生错误，请稍后重试！');
        }
      },
    },
  }
  </script>
  
  <style>
  .log-tag {
    color: green;
  }
  </style>
  