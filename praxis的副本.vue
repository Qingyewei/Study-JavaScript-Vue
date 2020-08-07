<template>
  <div>
    <h3>轮播图</h3>
    <div class="div" @mouseover="mouseover" @mouseleave="mouseleave">
      <div class="banner" :style="{left:imgLeft + 'px'}">
        <img :src="item"  v-for="(item , index) in imgArry" :key="index">
      </div>
      <div class="aaa">
        <ul v-for="(item , index) in imgArry" :key="index" >
          <li 
          v-if="index>0 && index<imgArry.length-1"
          @click="goZiel(index)">
          </li>
        </ul>
      </div>
      <div class="btn btn-left" @click="pre"><input type="button" value="&lt;"></div>
      <div class="btn btn-right" @click="next"><input type="button" value="&gt;"></div>
    </div>
    <el-button class="nextt" @click="goHome">返回</el-button>
  </div>
</template> 

<script>
export default {
  name: 'two',
  data() {
    return {
      imgArry: [
        "/static/imgs/1.jpg",
        "/static/imgs/2.jpg",
        "/static/imgs/3.jpg",
        "/static/imgs/4.jpg",
        "/static/imgs/5.jpg"
      ],
      imgLeft: -400,  //  left 值
      nowIndex: 1,  //  当前点击图片的下标值
      time: null,   //  动画定时器
      running: false,   //  是否处于动画状态(不能同时重复执行动画)
      dd:null,  //  自动播放定时器
      i: 100, //  一个动画帧数
      s: 2000, //  一个动画周期时间
    }
  },
   methods: {
     goHome() {
      this.$router.push("/");
    },
    pre() {
     this.animatione(-1,1)
    },
    next() {
      this.animatione(1, 1)
    },
    goZiel(index) {
      if(this.nowIndex != index){
        if(this.nowIndex > index) {
          this.animatione(-1,(this.nowIndex-index))
        }else{
          this.animatione(1,-(this.nowIndex-index))
        }
      }
    },
    animatione(n,m) {
      if(this.running){
        return
      }
      this.running = true
      var count = 0   // 当前动画已执行帧数
      var und = -400 * m * n
      this.nowIndex += n * m
      this.time = setInterval(()=> {
        this.imgLeft += und / this.i
        if(++count == this.i) {
          clearInterval(this.time)
          this.running = false
          if(this.nowIndex == 0){
            this.imgLeft = -(this.imgArry.length-2 ) * 400
            this.nowIndex = this.imgArry.length-2
          }
          if(this.nowIndex == this.imgArry.length-1){
            this.imgLeft = -400
            this.nowIndex = 1
          }
        }
      }, 20)
    },
    mouseleave() {
      this.fun();
    },
    mouseover() {
      this.cleater();
    },
    fun() {
      this.dd = setInterval(()=> {
        this.next()
      },10)
      
    },
    cleater() {
      clearInterval(this.dd)
    }
  },
   // 组件销毁前执行事件
  destroyed() {
    this.cleater();
  },
  mounted() {
    this.fun()
  },
  created() {
    this.imgArry.push(this.imgArry[0])
    this.imgArry.unshift(this.imgArry[this.imgArry.length-2])
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  list-style: none;
}
.div {
  width: 400px;
  height: 300px;
  margin: 100px auto;
  overflow: hidden;
  position: relative;
  border: 1px solid red;
}
.banner {
  width: 2800px;
  height: 300px;
  position: absolute;
}
.banner img {
  float: left;
}
.aaa {
  position: absolute;
  bottom: 0;
  left: 90px;
}
ul {
  float: left;
}
ul li {
  width: 15px;
  height: 15px;
  border: solid #bfa;
  border-radius: 50%;
  margin-left: 20px;
}
.btn {
  position: absolute;
  top: calc(50% - 25px);
}
.btn input[type="button"] {
  opacity: 0.2;
  font-size: 40px;
  border: none;
  color: #fff;
  background-color: #000;
}
.btn input[type="button"]:hover {
  color: #000;
  background-color: #fff;
  opacity: 0.8;
}
.btn-right {
  right: 0;
}
.nextt{
  position: absolute;
  right: 100px;
  font-size: 30px;
}
</style>