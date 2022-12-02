<template>
  <div class="home">
    <div class="header-class">
      <div class="left" @click="setIndex(-1)">
        <img :src="require('/public/image/icon/left.svg')"/>
      </div>
      <div>
        <img :src="require(`/public/image/icon/${IMG_LIST[imgIndex]}.svg`)"/>
      </div>
      <div class="right" @click="setIndex(1)">
        <img :src="require('/public/image/icon/right.svg')"/>
      </div>
    </div>
    <div class="name-class" contenteditable="true" @input="userInfo.name=$event.target.innerText">
      你的名字
    </div>
    <div class="start-class" @click="startPage">
      开始
    </div>
  </div>
</template>

<script setup>
import {ref} from "vue";
import {useRouter} from 'vue-router'
import {setPas} from "@/public/unit";

const router = useRouter();
let userInfo = ref({});
let imgIndex = ref(0);
const IMG_LIST = [
  "xiangsu_aixin",
  "xiangsu_biaoqing",
  "xiangsu_caomei",
  "xiangsu_fangzi",
  "xiangsu_jiangbei",
  "xiangsu_jinbi",
  "xiangsu_mao",
  "xiangsu_meiguihua",
  "xiangsu_pijiu",
  "xiangsu_qizhi",
  "xiangsu_shu",
  "xiangsu_tangguo",
  "xiangsu_tuzi",
  "xiangsu_wuqi",
  "xiangsu_xianrenzhang",
  "xiangsu_xiaoya",
  "xiangsu_xigua",
  "xiangsu_yaoshui",
  "xiangsu_youxiji",
  "xiangsu_zuanshi"
];
const setIndex = (index) => {
  if (imgIndex.value == 0 && index == -1) {
    imgIndex.value = IMG_LIST.length - 1;
  } else if (imgIndex.value == IMG_LIST.length - 1 && index == 1) {
    imgIndex.value = 0;
  } else {
    imgIndex.value += index
  }
}
const startPage = () => {
  userInfo.value['id'] = setPas(6);
  userInfo.value['name'] = userInfo.value.name || userInfo.value['id'];
  userInfo.value['image'] = IMG_LIST[imgIndex.value];
  router.push({
    path: "/about",
    query: userInfo.value
  })
}
</script>
<style lang="less" scoped>
.home {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  .header-class {
    display: flex;
    justify-content: center;
    align-items: center;

    .left, .right {
      cursor: pointer;
    }
  }

  .name-class {
    margin-top: 50px;
    color: white;
    font-size: 70px;
    font-size: 70px;
    background: #8c8c8c;
    border-radius: 100px;
    padding: 0px 40px;
  }

  .start-class {
    margin-top: 70px;
    font-size: 70px;
    color: white;
    border-radius: 10px;
    border: 4px #389e0d solid;
    background: #389e0d;
    height: 100px;
    line-height: 100px;
    padding: 0 10px;
    cursor: pointer;
  }

  .start-class:hover {
    background: #73d13d;
  }
}
</style>
