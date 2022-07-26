<template>
  <div id="app">
    <div class="container">


      <h1>Список каналов</h1>

      <div class="filter">
        <span 
        v-for="(category , index) in categories" 
        :key="index"
        :class="{active : category.velu == activeCategoriy }"
        @click="activeCategoriy = category.velu"
        >
         {{category.text}}
          </span>
      </div>

      <input type="text" placeholder="Search" v-model=" searChenal " >

      
      <div class="row"  v-if="this.filterChannels.length" >
        <a href="" class="row-item" v-for="(channel, index) in filterChannels" :key="index">   
          <img class="row-img" :src="channel.icon.url_500x500" :alt="channel.name">
          <div class="text">{{channel.name}}</div>
          <div class="badge" v-if="channel.input_quality !== 'sd'  ">{{channel.input_quality}} </div>
        </a>
      </div>
      <div class="loader"  v-else>
        <img src="@/assets/Spin-1s-200px.svg" alt="">

      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data(){
    return{
      channels: [],
      searChenal: "",
      activeCategoriy: "",
      categories:[
        {
          velu: "Все",
          text: "Все"
        },
          {
          velu: "Новостные",
          text: "Новостные"
        },
          {
          velu: "Кино",
          text: "Кино"
        },
          {
          velu: "Премиум",
          text: "Премиум"
        },
          {
          velu: "Познавательные",
          text: "Познавательные"
        },
          {
          velu: "Спорт",
          text: "Спорт"
        },
          {
          velu: "Музыка",
          text: "Музыка"
        },
          {
          velu: "Детские",
          text: "Детские"
        },
          {
          velu: "Узбекские",
          text: "Узбекские"
        },
      ]

    }
  },
  methods :{
    getChannels(){
      fetch('https://api.allplay.uz/api/v1/iptv/channels')
      .then(res=>{res.json().then(resjson=>{
        this.channels = resjson.data
      })

      })
    }
  },
  mounted() {
    this.getChannels()
  },
  computed:{
    filterChannels(){
      if(!this.activeCategoriy || this.activeCategoriy=="Все") return this.channels;
      return this.channels.filter((el)=>{
       return el.categories.some((cat) => cat.name == this.activeCategoriy);
      }).filter((el)=> {
        return el.name.toLowerCase().includes(this.searChenal.toLowerCase())
      })
    }
  }
};
</script>

<style>
@import "@/assets/css/style.css";
</style>
