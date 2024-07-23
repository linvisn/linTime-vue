<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import Cookies from 'js-cookie'

import SidePanelOption from "@/components/SidePanelOption.vue"
import MainPanel from "@/components/MainPanel.vue"
import Radio from "@/components/Radio.vue"

import Window from "@/components/Window/Window.vue"
import WindowOption from "@/components/Window/WindowOption.vue"
import WindowText from "@/components/Window/WindowText.vue"
import WindowLink from "@/components/Window/WindowLink.vue"


const defaultPanel = ref('')
const defaultFont = ref('')
const enabledFont = computed(() => ({
  'font-raleway': defaultFont.value === 'Raleway',
  'font-open-sans': defaultFont.value === 'Open Sans',
  'font-ubuntu': defaultFont.value === 'Ubuntu',
  'font-noto-serif': defaultFont.value === 'Noto Serif'
}))


onMounted(() => {
  if(Cookies.get('defaultPanel') == undefined) {
    Cookies.set('defaultPanel', mainPanels.value[0].title, { expires: 365 })
  }
  defaultPanel.value = Cookies.get('defaultPanel')

  let MP = mainPanels.value.find(item => item.title === defaultPanel.value)
  if (MP) {
    MP.active = true
  }

  if(Cookies.get('defaultFont') == undefined) {
    Cookies.set('defaultFont', fonts.value[0].family, { expires: 365 })
  }
  defaultFont.value = Cookies.get('defaultFont')
})


watch(defaultPanel, () => {
  Cookies.set('defaultPanel', defaultPanel.value, { expires: 365 })
})
watch(defaultFont, () => {
  Cookies.set('defaultFont', defaultFont.value, { expires: 365 })
})


const mainPanels = ref([
  { title: 'Timer', icon: "bi-hourglass-split", MP: "MPTimer"},
  { title: 'Stopwatch', icon: "bi-stopwatch", MP: "MPStopwatch"},
  { title: 'Tabata', icon: "bi-repeat", MP: "MPTabata" }
])
const windows = ref([
  { title: 'Settings', icon: "bi-gear-fill", window: "Settings"},
  { title: 'About', icon: "bi-info-circle-fill", window: "About"}
])
const fonts = ref([
  { family: 'Raleway', class: 'font-raleway'},
  { family: 'Open Sans', class: 'font-open-sans'},
  { family: 'Ubuntu', class: 'font-ubuntu'},
  { family: 'Noto Serif', class: 'font-noto-serif'}
])


const switchMPState = (event, id) => {
  document.querySelector(".active").classList.remove('active')
  event.target.classList.add("active")

  document.querySelector(".main-panel-active").classList.remove('main-panel-active')
  document.getElementById(id).classList.add('main-panel-active')
}

const switchWindow = (current, next) => {
  let currentEl = document.getElementById(current)
  currentEl.classList.toggle('window-active')

  if(next) {
    let nextEl = document.getElementById(next)
    nextEl.classList.toggle('window-active')
  }
}
</script>

<template>
<div :class="enabledFont">
  <window :id="'Settings'" :icon="'bi-gear-fill'" :windowType="'settingsWindow'" @switchWindow="switchWindow">
    <ul>
      <WindowOption @click="switchWindow('Settings', 'Customization')"><i class="bi bi-palette-fill"></i> Customization</WindowOption>
      <WindowOption @click="switchWindow('Settings', 'Tools')"><i class="bi bi-tools"></i> Used Tools</WindowOption>
      <a href="https://github.com/linvisn/linTime/" target="_blank"><WindowOption><i class="bi bi-github"></i> GitHub Repo</WindowOption></a>
    </ul>
  </window>

  <window :id="'About'" :icon="'bi-info-circle-fill'" :windowType="'aboutWindow'" @switchWindow="switchWindow">
    <ul>
      <WindowText>Just a simple app with usual and tabata timers and stopwatch, written in <WindowLink link="https://vuejs.org/">Vue.js 3</WindowLink> <i class="devicon-vuejs-plain"></i>. That's all I can say actually.</WindowText>
      <WindowText>Made by <WindowLink link="https://github.com/linvisn">linvisn</WindowLink> <i class="bi bi-github"></i> on July 14th 2024</WindowText>
    </ul>
  </window>

  <window :id="'Customization'" :prevId="'Settings'" :icon="'bi-palette-fill'" :windowType="'customizationWindow'" @switchWindow="switchWindow">
    <ul>
      <WindowText>Choose default panel:</WindowText>
      <Radio v-for="(mainPanel, index) in mainPanels" :key="index" :name="'defaultPanel'" :id="'defaultPanel' + index" v-model="defaultPanel" :value="mainPanel.title" />

      <WindowText>Choose default font:</WindowText>
      <Radio v-for="(font, index) in fonts" :key="index" :name="'defaultFont'" :class='font.class' :id="'defaultFont' + index" v-model="defaultFont" :value="font.family" />
    </ul>
  </window>

  <window :id="'Tools'" :prevId="'Settings'" :icon="'bi-tools'" :windowType="'toolsWindow'" @switchWindow="switchWindow">
    <ol>
      <li><WindowText><WindowLink link="https://vuejs.org/">Vue.js 3</WindowLink> <i class="devicon-vuejs-plain"></i></WindowText></li>
      <li><WindowText><WindowLink link="https://getbootstrap.com/">Bootstrap 5</WindowLink> <i class="devicon-bootstrap-plain"></i></WindowText></li>
      <li><WindowText><WindowLink link="https://icons.getbootstrap.com/">Bootstrap Icons</WindowLink> <i class="devicon-bootstrap-plain"></i></WindowText></li>
      <li><WindowText><WindowLink link="https://devicon.dev/">Devicon</WindowLink> <i class="devicon-devicon-plain"></i></WindowText></li>
      <li><WindowText><WindowLink link="https://fonts.google.com/">Google Fonts</WindowLink> <i class="devicon-google-plain"></i></WindowText></li>
      <li><WindowText><WindowLink link="https://www.npmjs.com/package/js-cookie">js-cookie</WindowLink> <i class="bi bi-cookie"></i></WindowText></li>
    </ol>
  </window>

  
  <div class="row">
    <div class="panel col-2">
      <div class="side-panel">
        <ul class="side-panel-menu">
          <SidePanelOption v-for="(option, index) in mainPanels" :key="index" :title="option.title" :icon="option.icon" :active="option.active" :MP="option.MP" @switchMPState="switchMPState" />
        </ul>
      </div>

      <div class="side-panel justify-content-end">
        <ul class="side-panel-menu">
          <SidePanelOption v-for="(option, index) in windows" :key="index" :title="option.title" :icon="option.icon" :window="option.window" @switchWindow="switchWindow" />
        </ul>
      </div>
    </div>

    <div class="panel col">
      <MainPanel v-for="(mainPanel, index) in mainPanels" :key="index" :id="mainPanel.MP" :title="mainPanel.title" :active="mainPanel.active" />
    </div>
  </div>
</div>
</template>

<style scoped>
.font-raleway {
  font-family: 'Raleway';
}
.font-open-sans {
  font-family: 'Open Sans';
}
.font-ubuntu {
  font-family: 'Ubuntu';
}
.font-noto-serif {
  font-family: 'Noto Serif';
}
</style>