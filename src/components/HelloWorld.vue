<template>
  <div>
    <h1>{{ msg }}</h1>
    <h2>Essential Links hello 123</h2>
    <img :src="png1" width="80" height="80" />
    <img :src="png2" width="50" height="50" />
    <br />
    <canvas ref="canvas" class="canvas" width="100%" height="100%"></canvas>
    <img :src="infactQrCode" alt style="width: 100%;height:100%" />
    <br/>
    <div id="qrcode"></div>

  </div>
</template>

<script>
import imgbk from "@/assets/t1.png";
import imgqr from "@/assets/t2.png";
import QRCode from 'qrcodejs2'


export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      infactQrCode: "",
      png1:imgbk,
      png2:imgqr,
    };
  },
  methods: {
    drawcanvas() {
      console.log("进入构建图片");
      var img1 = new Image();
      img1.src = imgbk;
      //png1=img1.src;
      console.log("img1.src:"+img1.src)

      let canvas = this.$refs.canvas;
      canvas.width = 2880
      canvas.height = 1280
      // 获取上下文
      let con = canvas.getContext("2d");

      img1.onload = function () {
        console.log("img1onload:"+img1.src)
        con.drawImage(img1, 0, 0, 2880, 1280);

      let myCanvas = document.getElementById('qrcode').getElementsByTagName('canvas');
      let imgURL=myCanvas[0].toDataURL('image/jpg');

      var img2 = new Image();
      // img2.src = imgqr;
      img2.src = imgURL;

        img2.onload = function () {
          
          console.log("img2onload:"+img2.src)

          con.drawImage(img2, 448, 348, 268, 268);
          console.log("geturl:"+canvas.toDataURL("image/png"))
          this.infactQrCode = canvas.toDataURL("image/png");
        };
      };
    },
     qrcode () {
            let that = this;
            let qrcode = new QRCode('qrcode', {
                width: 200,
                height: 200,        // 高度
                text:  'http://127.0.0.1:8080/test/server',   // 二维码内容
                // render: 'canvas' ,   // 设置渲染方式（有两种方式 table和canvas，默认是canvas）
                // background: '#f0f',   // 背景色
                // foreground: '#ff0'    // 前景色
            })
        },
  },
  mounted() {
    this.drawcanvas();
    this.qrcode();
  },
};
</script>

<style scoped>
.hello {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
h1,
h2 {
  font-weight: normal;
}

.canvas {
  top: 0;
  left: 0;
  margin: 0 auto;
  width: 800px;
  height: 840px;
  z-index: 100;
}
</style>
