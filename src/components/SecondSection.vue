<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
const tabs = [
  {
    key: 'cartoon',
    title: '动画'
  },
  {
    key: 'food',
    title: '美食'
  },
  {
    key: 'movie',
    title: '电影'
  },
  {
    key: 'life',
    title: '生活'
  }
]
const TAB_HEIGHT = 56
const getImageUrl: (name: string)=>string = name => new URL(`../assets/${name}${name !== 'movie' ? '.jpg' : '.png'}`, import.meta.url).href
const activeTab = ref('cartoon')
const secondSection = ref<HTMLElement | null>(null)
const tabContents = ref<HTMLElement[] | null>(null)
const isFixed = ref(false)
const setFixed = (bool: boolean) => isFixed.value = bool
const activate: (key: string)=>void = key => {
  activeTab.value = key
  const index = tabs.findIndex(item => item.key === key)
  tabContents.value![index].scrollIntoView({ behavior: 'smooth' })
}
const onScroll = () => {
  if (secondSection.value) {
    const { top } = secondSection.value.getBoundingClientRect()
    setFixed(top <= 0)
    tabContents.value?.forEach((tabContent, index) => {
      const { top } = tabContent.getBoundingClientRect()
      const { key } = tabs[index]
      if (Math.floor(top) <= TAB_HEIGHT) {
        activeTab.value = key
      }
    })
  }
  
}
onMounted(() => {
  window.addEventListener('scroll', onScroll)
})
onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
})
</script>

<template>
  <div class="second-section" ref="secondSection">
    <!-- tab -->
    <ul :class="{'fixed': isFixed}">
      <li v-for="tab in tabs" :key="tab.key" @click="activate(tab.key)">
        <span>{{ tab.title }}</span>
        <span class="line" :class="{'visible': activeTab === tab.key}"></span>
      </li>
    </ul>
    <!-- tab content -->
    <div>
      <section v-for="tab in tabs" :key="tab.key" ref="tabContents">
        <h2>{{ tab.title }}</h2>
        <img :src="getImageUrl(tab.key)" :alt="tab.key">
      </section>
    </div>
    <!-- bottom -->
    <div class="btn-wrapper" :class="{'visible': isFixed}">
      <img src="../assets/logo.png" alt="LOGO">
      <a href="https://d.bilibili.com/download_app.html">
        <button>在 APP 中打开</button>
      </a>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.second-section {
  padding-top: 56px;
  position: relative;
  ul {
    position: absolute;
    top: 0;
    width: 100%;
    height: 56px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    background: #e36c3f;
    &.fixed {
      position: fixed;
    }
    li {
      position: relative;
      color: white;
      list-style: none;
      .line {
        position: absolute;
        left: 50%;
        bottom: -8px;
        transform: translateX(-50%);
        width: 100%;
        height: 4px;
        background: green;
        opacity: 0;
        transition: opacity .3s;
        &.visible {
          opacity: 1;
        }
      }
    }
  }
  section {
    padding: 16px;
    scroll-margin: 56px;
    h2 {
      color: white;
    }
    img {
      width: 100%;
      margin-top: 16px;
      border-radius: 6px;
    }
  }
  .btn-wrapper {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 64px;
    padding: 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: linear-gradient(0deg, rgba(255,255,255,1) 0%, rgba(255,255,255,0) 100%);
    opacity: 0;
    transform: translateY(64px);
    transition: opacity .3s, transform .3s;
    &.visible {
      opacity: 1;
      transform: translateY(0);
    }
    img {
      width: 100px;
    }
    button {
      border: none;
      padding: 8px 16px;
      border-radius: 50px;
      color: white;
      background: #fb7299;
    }
  }
}
</style>