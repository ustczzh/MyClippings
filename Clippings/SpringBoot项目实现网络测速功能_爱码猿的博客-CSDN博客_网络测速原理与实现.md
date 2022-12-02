# SpringBoot项目实现网络测速功能_爱码猿的博客-CSDN博客_网络测速原理与实现
上行速度：原理就是得到客户端的[时间戳](https://so.csdn.net/so/search?q=%E6%97%B6%E9%97%B4%E6%88%B3&spm=1001.2101.3001.7020)，然后向服务端发起POST请求（请求体尽量大）请求成功的时候执行回调函数，从服务端返回服务端接收到请求时候的时间戳和contentLength（单位字节Byte），再利用contentLength除以服务端时间和客户端时间差（注意需要将时间戳的单位ms转为s）就得到标准的每秒请求多少字节

根据国家宽带速率，需要乘以8，所以得到Bps。然后再根据单位换算得到响应的Kbps、Mbps。

1Byte=8Bits （1字节=8字）

1Kb=1024Byte （1Kb=1024B）

1Mb=1024Kb （1Mb=1024Kb）

下载速度：定义Image对象，给Image对象指定图片资源的地址（可以是本地也可以是远程服务器），同时你必须要手动指定该图片的大小（单位Byte字节）仍然是需要获得发起请求该图片资源的时间戳。onload钩子会在图片加载完成的时候执行后面这个函数，所以我们需要在该函数里执行结束时的相应操作，比如得到完成下载时候的时间戳等…

后端只需要提供一个上传接口即可

```java
@PostMapping("/uploadSpeedTest")
    public Map<String,Object> uploadSpeedTest(HttpServletRequest request){
        
        long contentLength = request.getContentLengthLong();
        log.info("contentLength->[{} Byte]",contentLength);

        
        HashMap<String, Object> dataMap = new HashMap<>();
        dataMap.put("contentLength",contentLength);

        
        HashMap<String, Object> resMap = new HashMap<>();
        resMap.put("msg","客户端到服务端请求");
        resMap.put("data",dataMap);

        return resMap;
    }

```

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>
    网络测速
  </title>
  <meta name="renderer" content="webkit">
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="format-detection" content="telephone=no">
  
  <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
  <style>
    .el-header, .el-footer {
      
      color: #333;
      text-align: center;
      line-height: 60px;
    }

    .el-main {
      
      color: #333;
      text-align: center;
      
    }

    body > .el-container {
      margin-bottom: 40px;
    }

    .el-row{
      margin: 15px;
    }
  </style>
</head>
<body>

<div id="app">
  <el-container>
    <el-main>

      
      <el-progress
              type="circle"
              :show-text="count===maxCount || isError"
              :percentage="count"
              :width="200"
              :stroke-width="10"
              :status="count | statusFilter(maxCount,isError) "></el-progress>
      <el-row el-row :gutter="10">
        <el-col
                :xs="{span: 24,offset: 0}"
                :sm="{span: 12,offset: 6}"
                :md="{span: 8,offset: 8}">
          <el-input :value="speedArray | aveSpeedFilter" readonly>
            <template slot="prepend">平均速率</template>
          </el-input>
        </el-col>
      </el-row>
      <el-row el-row :gutter="10">
        <el-col
                :xs="{span: 24,offset: 0}"
                :sm="{span: 12,offset: 6}"
                :md="{span: 8,offset: 8}">
          <el-input :value="speedArray | maxSpeedFilter" readonly>
            <template slot="prepend">最大速率</template>
          </el-input>
        </el-col>
      </el-row>
      <el-button
              @click="downloadButton"
              :disabled="flag"
              type="primary"
              :loading="flag"
              size="medium">测试下载速度
      </el-button>
      <el-button
              @click="uploadButton"
              :disabled="flag"
              type="success"
              :loading="flag"
              size="medium">测试上行速度
      </el-button>
    </el-main>
  </el-container>
</div>


<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<script src="https://unpkg.com/element-ui/lib/index.js"></script>

<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.0/jquery.js"></script>
<script>
  let app=new Vue({
    el: '#app',
    data() {
      return {
        msg: 'HelloVuejs!',
        count: 0,         
        maxCount: 100,    
        speedArray:[],    
        flag: false,      
        waitTime: 300,    
        isError: false    
      }
    },
    filters: {
      
      statusFilter(status,maxCount,isError){
        if (status===maxCount) return 'success';
        if (isError) return 'exception';
      },
      
      aveSpeedFilter(speedArray) {
        if (speedArray.length===0) return ;
        let totalSpeed = 0;
        let number = speedArray.length;
        for (let speed of speedArray) {
          totalSpeed += speed;
        }
        let aveSpeed = totalSpeed/number;
        return aveSpeed.toFixed(2)+'Mbps';
      },
      
      maxSpeedFilter(speedArray){
        if (speedArray.length===0) return ;
        let maxSpeed = 0;
        for (let speed of speedArray) {
          if (speed>maxSpeed) maxSpeed = speed;
        }
        return maxSpeed.toFixed(2)+'Mbps';
      },
    },
    methods: {


      downloadButton(){
        console.log('开始测试下载速度->>>>');
        this.isError = false;
        this.count = 0;
        this.speedArray = [];
        this.flag = true;
        this.startDownload();
      },

      startDownload(){
        setTimeout(()=>{
          this.download();
        },this.waitTime)
        this.count+=10;
      },
      
      download(){
        let image = new Image(); 
        
        let imageSrc='./download/test.JPG';	
        let imageSize=7984555;			   
        image.src = imageSrc + '?n=' +Math.random(); 
        let startTime = new Date().getTime(); 
        let that = this;
        image.onload = function () { 
          let endTime = new Date().getTime(); 
          
          
          let diffSeconds = (endTime - startTime)/1000; 
          let bps = imageSize/diffSeconds;
          let speedBps = bps*8; 
          let speedKbps = speedBps / 1024;  
          let speedMbps = speedKbps / 1024; 
          console.log('['+that.count/10+']'+'下载速率',speedMbps,'Mbps');
          
          that.speedArray.push(speedMbps);
          delete image; 
          if (that.count<that.maxCount){
            that.startDownload();
          } else {
            that.flag = false;
          }
        };
      },


      
      uploadButton(){
        console.log('开始测试上行速度->>>>');
        this.isError = false;
        this.count = 0;
        this.speedArray = [];
        this.flag = true;
        this.startUpload();
      },
      
      startUpload(){
        setTimeout(()=>{
          this.upload();
        },this.waitTime)
        this.count+=20;
      },
      
      upload(){
        let startTime = new Date().getTime();
        
        let that = this;
        let text = 'A'; 
        let totalText ;
        for (let i = 0; i < 1024 * 1024 * 2; i++) {
          totalText+=text; 
        }
        let formData = new FormData();
        formData.append('text',totalText);
        $.ajax({
          url: '//localhost:8080/uploadSpeedTest',
          type: 'POST',
          timeout: 60000,
          processData:false,
          contentType:false,
          data:formData,
          success(res){
            let endTime = new Date().getTime();
            let diffSeconds = (endTime-startTime)/1000;
            let contentLength = res.data.contentLength;
            let bps = contentLength/diffSeconds;
            let speedBps = bps * 8;
            let speedKbps = speedBps / 1024 ;
            let speedMbps = speedKbps / 1024 ;
            console.log('['+that.count/20+']'+'上行速率',speedMbps,'Mbps（仅供参考）');
            
            that.speedArray.push(speedMbps);
            if (that.count<that.maxCount){
              that.startUpload();
            } else {
              that.flag = false;
            }
          },
          error(err){
            that.isError = true;
            that.flag = false;
            console.log(err);
          }
        })
      },


    },
  })
</script>

</body>
</html>

```

参考[网络测速demo开源项目](https://gitee.com/FanGaoXS/speed-test)