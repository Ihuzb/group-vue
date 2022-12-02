<template>
  <div class="about" @click="clickPage">
    <div></div>
    <div class="image-class shake-slow" v-for="item in imageList" :style="{left: `${item.x}px`,top:`${item.y}px`}">
      <img :src="require(`/public/image/icon/${item.image}.svg`)"/>
      <div class="text">
        {{ item.name?.substr(0, 10) }}
      </div>
    </div>
    <div ref="imageRef" class="image-class shake-chunk shake-constant">
      <img v-if="userInfo.image" :src="require(`/public/image/icon/${userInfo.image}.svg`)"/>
      <div class="text">
        {{ '> ' }}
        {{ userInfo.name?.substr(0, 5) }}
        {{ ' <' }}
      </div>
    </div>
  </div>
</template>
<script setup>
import {onMounted, ref} from "vue";
import {useRoute} from 'vue-router'

const router = useRoute();

let userInfo = ref({});
let imageList = ref([]);
let webSocket = new WebSocket(
    // 此处填写你要连接的ws地址
    "ws://127.0.0.1:8181"
)
const imageRef = ref(null);
const sendIndo = (info) => {
  webSocket.send(JSON.stringify(info));
}
const clickPage = (e) => {
  let {clientX, clientY} = e;
  console.log(clientX, clientY)
  imageRef.value.style.left = `${clientX - 35}px`;
  imageRef.value.style.top = `${clientY - 35}px`;
  sendIndo(Object.assign({x: clientX - 35, y: clientY - 35}, userInfo.value))
}
const setImage = (info) => {
  if (info && info.length) {
    imageList.value = info;
  }
}
//建立连接
webSocket.onopen = function () {
  console.log('通讯开始1');
  sendIndo({type: 1, ...userInfo.value})
}
//接收服务端消息
webSocket.onmessage = function (e) {
  console.log(e, 9999)
  let {data} = e;
  let {type, userInfo: userList} = JSON.parse(data);
  if (type == 1) {
    console.log('欢迎131312')
  } else if (type == 2) {
    delete userList[userInfo.value.id]
    setImage(Object.values(userList))
  }
}
onMounted(() => {
  userInfo.value = router.query;
  console.log(router.query, 1);

})
</script>
<style lang="less" scoped>
.about {
  height: 100%;
  position: relative;

  .image-class {
    width: 70px;
    position: absolute;

    img {
      width: 100%;
    }

    .text {
      font-size: 15px;
      color: white;
    }
  }
}
</style>