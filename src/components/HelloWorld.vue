<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="download">导出Excel</button>
  </div>
</template>

<script>
import Axios from 'axios'
import {exportExcel} from '../api/common/common.js'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Test EasyExcel'
    }
  },
  methods: {
    download(){
      Axios({
        method: 'get',// 设置请求方式
        url: `http://localhost:8082/export/excel`,// 设置请求地址
        data: this.exportForm,
        responseType: 'blob'
      }).then(function (res) {
        // 得到请求到的数据后，对数据进行处理
        let blob = new Blob([res.data], {type: 'application/vnd.ms-excel;charset=utf-8'});// 创建一个类文件对象：Blob对象表示一个不可变的、原始数据的类文件对象
        let fileName = decodeURI(res.headers['content-disposition']);// 设置文件名称,decodeURI：可以对后端使用encodeURI() 函数编码过的 URI 进行解码。encodeURI() 是后端为了解决中文乱码问题
         console.log(fileName)
        if (fileName) {// 根据后端返回的数据处理文件名称
          fileName = fileName.substring(fileName.indexOf('=') + 1);
        }
        const link = document.createElement('a')// 创建一个a标签
        link.download = fileName;// 设置a标签的下载属性
        link.style.display = 'none';// 将a标签设置为隐藏
        link.href = URL.createObjectURL(blob);// 把之前处理好的地址赋给a标签的href
        document.body.appendChild(link);// 将a标签添加到body中
        link.click();// 执行a标签的点击方法
        URL.revokeObjectURL(link.href) // 下载完成释放URL 对象
        document.body.removeChild(link)// 移除a标签
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
