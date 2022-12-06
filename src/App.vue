<template>
<v-app id="app">
  <v-app-bar
    app
    color="white"
    flat

  >
    <template v-slot:extension>
      <v-slider
          hint=""
          v-model="barWidth"
        ></v-slider>
      <span>Ширина: {{barWidth}}%</span>
     </template>
    <v-row>
      <v-btn
        elevation="2"
        class="ma-auto"
        @click="genTags()"
      >Обновить TAGS</v-btn>
      
      <v-radio-group
        class="ma-auto"
        v-model="radios"
        row
      >
        <v-radio
          label="center"
          value="center"
        ></v-radio>
        <v-radio
          label="left"
          value="left"
        ></v-radio>
      </v-radio-group>
      </v-row>
  </v-app-bar>

  <v-main class="grey lighten-3">
    <v-container>
      <v-row>
        
        <v-col
          class="d-flex justify-center"

        >
          <v-sheet
            min-height="30vh"
            :width = "barWidth+'%'"
            rounded
          >
            <TagBar :tags="tags" :center="radios == 'center'" :left="radios == 'left'"/>
          </v-sheet>
        </v-col>

        
      </v-row>
      <v-row>
        <v-col
          class="d-flex justify-center"
        >
        TAGS: {{tags}}
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</v-app>
</template>

<script>
import TagBar from './components/TagBar.vue'

export default {
  name: 'App',
  data: () => ({
      barWidth: 50,
      tags: [],
      radios: 'center'
    }),
  components: {
    TagBar
  },
  created(){
    this.init();
  },
  methods:{
    init() { // Задать массив с тэгами
      this.genTags()
    },
    genTags() {
      this.tags=[]
      const iconArr = ['mdi-domain', 'mdi-message-text', 'mdi-dialpad', 'mdi-email', 'mdi-call-split', 'mdi-arrow-up-bold-box-outline']
      const textArr = ['Практический', 'опыт показывает', 'что начало', 'повседневной', 'работы', 'по формированию позиции']
      const getRandomInt = (max)=>Math.floor(Math.random() * max)  
      for (var i = 0;i< getRandomInt(10); i++) {
        const text = textArr[getRandomInt(textArr.length - 1)];
        const icon = iconArr[getRandomInt(iconArr.length - 1)]
        getRandomInt(2) == 1 ?  this.tags.push({text}) :  this.tags.push({text, icon})
        }
    },
    
  }
}

</script>

<style>

</style>
