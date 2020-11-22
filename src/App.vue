<template>
  <div>
    <img alt="Vue logo" src="./assets/logo.png">
    <h2>欢迎光临红浪漫西域中心</h2>
    <div>请选择一位美女为您服务</div>
    <div>
      <button v-for="(item, index) in girls" v-bind:key="index" @click="SelectGirl(index)">{{ index }}:{{ item }}</button>
    </div>
    <div>您选择了【{{ selectedGirl }}】为您服务</div>
    <div>
      <button @click="OverAction()">点餐完毕</button>
    </div>
    <div>{{ overText }}</div>
    <div>
      <div>{{ nowTime }}</div>
      <button @click="GetNowTime()">获取当前时间</button>
    </div>
    <div class="divImg">
      <div v-if="loading">
      Loading....
      </div>
      <img v-if="loaded" :src="result.imgUrl">
    </div>
    <modal />
    <div>
      <Suspense>
        <template #default>
          <AsyncShow />
        </template>
        <template #fallback>
          <h1>Loading...</h1>
        </template>
      </Suspense>
    </div>
    <div>
      <Suspense>
        <template #default>
          <girl-show />
        </template>
        <template #fallback>
          <h1>Loading...</h1>
        </template>
      </Suspense>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, reactive, toRefs, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onRenderTriggered, onErrorCaptured, watch } from 'vue';
import modal from "./components/Modal.vue"
import { nowTime, GetNowTime } from "./hooks/UseNowTime";
import UseUrlAxios from "./hooks/UseURLAxios";
import AsyncShow from "./components/AsyncShow.vue";
import GirlShow from "./components/GirlShow.vue";

interface DataProps {
  girls: string[];
  selectedGirl: string;
  SelectGirl: (index: number) => void;
}

export default {
  name: "App",
  components: {
    modal,
    AsyncShow,
    GirlShow
  },
  setup() {
    console.log("开始创建组件——setup()");
    const data: DataProps = reactive({
      girls: ["大脚", "刘英", "晓红"],
      selectedGirl: "",
      SelectGirl: (index: number) => {
        data.selectedGirl = data.girls[index];
      }
    });

    onBeforeMount(() => {
      console.log("组件挂载到页面之前执行——onBeforeMount()");
    });

    onMounted(() => {
      console.log("组件挂载到页面之后执行——onMounted()");
    });


    onBeforeUpdate(() => {
      console.log("组件更新之前执行——onBeforeUpdate()");
    });

    onUpdated(() => {
      console.log("组件更新之后执行——onUpdated()");
    });

    // onRenderTracked((event) => {
    //   console.log("状态跟踪钩子函数");
    //   console.log(event);
    // });

    onRenderTriggered((event) => {
      console.log("状态跟踪钩子函数");
      console.log(event);
    });
    
    onErrorCaptured((error) => {
      console.log(`error ==>${error}`);
      return true;
    });

    const refData = toRefs(data);
    const overText = ref("红浪漫");
    const OverAction = () => {
      overText.value = "点餐完成：" + overText.value;
    };

    watch([overText, () => data.selectedGirl], (newValue, oldValue) => {
      console.log(`newValue:${newValue}`);
      console.log(`oldValue:${oldValue}`);
      document.title = newValue[0];
    });

    const { result, loading, loaded } = UseUrlAxios("https://apiblog.jspang.com/default/getGirl");

    return {
      ...refData,
      overText,
      OverAction,
      nowTime,
      GetNowTime,
      result,
      loading,
      loaded,
      AsyncShow
    };
  }
};
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
</style>
