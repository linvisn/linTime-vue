<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import Cookies from 'js-cookie'

import SidePanelOption from "@/components/SidePanelOption.vue"
import MainPanel from "@/components/MainPanel.vue"
import Radio from "@/components/Radio.vue"
import Switch from "@/components/Switch.vue"

import Offcanvas from './components/Offcanvas/Offcanvas.vue'
import OffcanvasOption from "@/components/Offcanvas/OffcanvasOption.vue"
import OffcanvasText from "@/components/Offcanvas/OffcanvasText.vue"
import OffcanvasLink from "@/components/Offcanvas/OffcanvasLink.vue"


const defaultPanel = ref('')
const defaultFont = ref('')
const settingsOptionsExpanded = ref('')
const defaultTheme = ref('')

const enabledFont = computed(() => ({
  'font-raleway': defaultFont.value === 'Raleway',
  'font-open-sans': defaultFont.value === 'Open Sans',
  'font-ubuntu': defaultFont.value === 'Ubuntu',
  'font-noto-serif': defaultFont.value === 'Noto Serif',
  'font-jetbrains-mono': defaultFont.value === 'JetBrains Mono',
  'font-source-code-pro': defaultFont.value === 'Source Code Pro',
  'font-geologica': defaultFont.value === 'Geologica'
}))
const chosenTheme = computed(() => ({
  'midnight-fireplace': defaultTheme.value === 'Midnight Fireplace'
}))

window.addEventListener('beforeunload', (event) => {
  mainPanels.value.forEach(mainPanel => {
    if(mainPanel.isStarted) {
      event.preventDefault()
    }
  })
})


const mainPanels = ref([
  { title: 'Timer', icon: "bi-hourglass-split", MP: "MPTimer", isStarted: false},
  { title: 'Stopwatch', icon: "bi-stopwatch", MP: "MPStopwatch", isStarted: false},
  { title: 'Tabata', icon: "bi-repeat", MP: "MPTabata", isStarted: false}
])
const offcanvas = ref([
  { title: 'Settings', icon: "bi-gear-fill", href: 'settingsOffcanvas'},
  { title: 'About', icon: "bi-info-circle-fill", href: 'aboutOffcanvas'}
])
const fonts = ref([
  { family: 'Raleway', class: 'font-raleway' },
  { family: 'Open Sans', class: 'font-open-sans' },
  { family: 'Ubuntu', class: 'font-ubuntu' },
  { family: 'Noto Serif', class: 'font-noto-serif' },
  { family: 'JetBrains Mono', class: 'font-jetbrains-mono' },
  { family: 'Source Code Pro', class: 'font-source-code-pro'},
  { family: 'Geologica', class: 'font-geologica'}
])
const themes = ref([
  { title: 'Default' },
  { title: 'Midnight Fireplace' }
])

const cookies = ref([
  { variable: defaultPanel, title: 'defaultPanel', defaultValue: mainPanels.value[0].title },
  { variable: defaultFont, title: 'defaultFont', defaultValue: fonts.value[0].family },
  { variable: settingsOptionsExpanded, title: 'settingsOptionsExpanded', defaultValue: true },
  { variable: defaultTheme, title: 'defaultTheme', defaultValue: 'Default' }
])


onMounted(() => {
  cookies.value.forEach(cookie => {
    if(Cookies.get(cookie.title) == undefined) {
      Cookies.set(cookie.title, cookie.defaultValue, { expires: 365 })
    }

    if(cookie.title === 'settingsOptionsExpanded') {
      cookie.variable = Cookies.get(cookie.title) === 'true'
    }
    else {
      cookie.variable = Cookies.get(cookie.title)
    }
  })

  let MP = mainPanels.value.find(item => item.title === defaultPanel.value)
  MP.active = true
})


cookies.value.forEach(cookie => {
  watch(() => cookie.variable, (newValue) => {
    Cookies.set(cookie.title, newValue, { expires: 365 })
  })
})


const switchMPState = (event, id) => {
  document.querySelector(".active").classList.remove('active')
  event.target.classList.add("active")

  document.querySelector(".main-panel-active").classList.remove('main-panel-active')
  document.getElementById(id).classList.add('main-panel-active')
}
</script>

<template>
<div :class="enabledFont">
  <Offcanvas :id="'settingsOffcanvas'" :title="'Settings'" :icon="'bi-gear-fill'">
    <OffcanvasOption data-bs-toggle="offcanvas" href="#customizationOffcanvas" aria-controls="customizationOffcanvas"><i class="bi bi-palette-fill"></i> Customization</OffcanvasOption>
    <OffcanvasOption data-bs-toggle="offcanvas" href="#toolsOffcanvas" aria-controls="toolsOffcanvas"><i class="bi bi-tools"></i> Used Tools</OffcanvasOption>
    <a href="https://github.com/linvisn/linTime/" target="_blank"><OffcanvasOption><i class="bi bi-github"></i> GitHub Repo</OffcanvasOption></a>
  </Offcanvas>

  <Offcanvas :id="'aboutOffcanvas'" :title="'About'" :icon="'bi-info-circle-fill'">
    <OffcanvasText>A simple app with usual Timer, Tabata Timer and Stopwatch, written in <OffcanvasLink link="https://vuejs.org/">Vue.js 3</OffcanvasLink> <i class="devicon-vuejs-plain"></i></OffcanvasText>
    <OffcanvasText>Made by <OffcanvasLink link="https://github.com/linvisn">linvisn</OffcanvasLink> <i class="bi bi-github"></i> on July 14th 2024</OffcanvasText>
  </Offcanvas>

  <Offcanvas :id="'customizationOffcanvas'" :title="'Customization'" :icon="'bi-palette-fill'" :prevOffcanvas="'settingsOffcanvas'">
    <Switch :text="'Set Settings options expanded'" v-model="settingsOptionsExpanded" />
    <OffcanvasText>Choose default panel:</OffcanvasText>
    <div v-if="settingsOptionsExpanded">
      <Radio v-for="(mainPanel, index) in mainPanels" :key="index" :name="'defaultPanel'" :id="'defaultPanel' + index" v-model="defaultPanel" :value="mainPanel.title" />
    </div>
    <select v-else class="form-select" v-model="defaultPanel">
      <option v-for="(mainPanel, index) in mainPanels" :key="index" :name="'defaultPanel'" :id="'defaultPanel' + index" :value="mainPanel.title">{{ mainPanel.title }}</option>
    </select>

    <OffcanvasText>Choose default font:</OffcanvasText>
    <div v-if="settingsOptionsExpanded">
      <Radio v-for="(font, index) in fonts" :key="index" :name="'defaultFont'" :class='font.class' :id="'defaultFont' + index" v-model="defaultFont" :value="font.family" />
    </div>
    <select v-else class="form-select" v-model="defaultFont">
      <option v-for="(font, index) in fonts" :key="index" :name="'defaultFont'" :class='font.class' :id="'defaultFont' + index" :value="font.family">{{ font.family }}</option>
    </select>
    
    <OffcanvasText>Choose default theme:</OffcanvasText>
    <div v-if="settingsOptionsExpanded">
      <Radio v-for="(theme, index) in themes" :key="index" :name="'theme'" :id="'theme' + index" v-model="defaultTheme" :value="theme.title" />
    </div>
    <select v-else class="form-select" v-model="defaultTheme">
      <option v-for="(theme, index) in themes" :key="index" :name="'theme'" :id="'theme' + index" :value="theme.title">{{ theme.title }}</option>
    </select>
  </Offcanvas>
  
  <Offcanvas :id="'toolsOffcanvas'" :title="'Tools'" :icon="'bi-tools'" :prevOffcanvas="'settingsOffcanvas'">
    <OffcanvasText><OffcanvasLink link="https://vuejs.org/">Vue.js 3</OffcanvasLink> <i class="devicon-vuejs-plain"></i></OffcanvasText>
    <OffcanvasText><OffcanvasLink link="https://getbootstrap.com/">Bootstrap 5</OffcanvasLink> <i class="devicon-bootstrap-plain"></i></OffcanvasText>
    <OffcanvasText><OffcanvasLink link="https://icons.getbootstrap.com/">Bootstrap Icons</OffcanvasLink> <i class="devicon-bootstrap-plain"></i></OffcanvasText>
    <OffcanvasText><OffcanvasLink link="https://devicon.dev/">Devicon</OffcanvasLink> <i class="devicon-devicon-plain"></i></OffcanvasText>
    <OffcanvasText><OffcanvasLink link="https://fonts.google.com/">Google Fonts</OffcanvasLink> <i class="devicon-google-plain"></i></OffcanvasText>
    <OffcanvasText><OffcanvasLink link="https://www.npmjs.com/package/js-cookie">js-cookie</OffcanvasLink> <i class="bi bi-cookie"></i></OffcanvasText>
  </Offcanvas>

  
  <div class="row">
    <div class="panel col-2">
      <div class="side-panel" :class="chosenTheme">
        <div>
          <SidePanelOption v-for="(option, index) in mainPanels" :key="index" :title="option.title" :icon="option.icon" :active="option.active" :MP="option.MP" @switchMPState="switchMPState" :class="chosenTheme" />
        </div>
        <div>
          <SidePanelOption v-for="(option, index) in offcanvas" :key="index" :title="option.title" :icon="option.icon" data-bs-toggle="offcanvas" :href="'#' + option.href" :aria-controls="option.href" :class="chosenTheme" />
        </div>
    </div>
  </div>

    <div class="panel col">
      <MainPanel v-for="(mainPanel, index) in mainPanels" :key="index" :id="mainPanel.MP" :title="mainPanel.title" :active="mainPanel.active" v-model:isStarted="mainPanel.isStarted" :class="chosenTheme" />
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
.font-jetbrains-mono {
  font-family: 'JetBrains Mono';
}
.font-source-code-pro {
  font-family: 'Source Code Pro';
}
.font-geologica {
  font-family: 'Geologica';
}
</style>