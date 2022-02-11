<template>
  <p class="text-5xl md:text-7xl font-title text-[#82CCDD] text-center pt-20">AniSearch</p>
  <form @submit.prevent="carianime">
      <div class="flex justify-center py-11">
      <input type="text" v-model="cari" placeholder="Search Anime Here" class="py-2 px-3 w-[270px] md:w-[400px] rounded-md focus:outline-[#307b8b] font-medium">
      <button class="py-2 px-3 bg-[#82CCDD] rounded-md ml-4">
          <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
width="27" height="27"
viewBox="0 0 50 50"
style=" fill:#000000;"><path d="M 21 3 C 11.601563 3 4 10.601563 4 20 C 4 29.398438 11.601563 37 21 37 C 24.355469 37 27.460938 36.015625 30.09375 34.34375 L 42.375 46.625 L 46.625 42.375 L 34.5 30.28125 C 36.679688 27.421875 38 23.878906 38 20 C 38 10.601563 30.398438 3 21 3 Z M 21 7 C 28.199219 7 34 12.800781 34 20 C 34 27.199219 28.199219 33 21 33 C 13.800781 33 8 27.199219 8 20 C 8 12.800781 13.800781 7 21 7 Z"></path></svg>
      </button>
      </div>
  </form>

  <div class="container mx-auto">
      <template v-if="loading">
      <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="margin: auto; background: none; display: block; shape-rendering: auto;" width="200px" height="200px" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid"><rect x="17.5" y="30" width="15" height="40" fill="#93dbe9"><animate attributeName="y" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="18;30;30" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.2s"></animate><animate attributeName="height" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="64;40;40" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.2s"></animate></rect><rect x="42.5" y="30" width="15" height="40" fill="#689cc5"><animate attributeName="y" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="20.999999999999996;30;30" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.1s"></animate><animate attributeName="height" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="58.00000000000001;40;40" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.1s"></animate></rect><rect x="67.5" y="30" width="15" height="40" fill="#5e6fa3"><animate attributeName="y" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="20.999999999999996;30;30" keySplines="0 0.5 0.5 1;0 0.5 0.5 1"></animate><animate attributeName="height" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="58.00000000000001;40;40" keySplines="0 0.5 0.5 1;0 0.5 0.5 1"></animate></rect></svg>
      </template>
      <template v-else>          
      <div class="grid grid-cols-1 lg:grid-cols-4 md:grid-cols-3 xl:grid-cols-5 justify-items-center gap-y-5 mb-16 xl:gap-x-5">
          <div class="w-[250px] bg-white rounded hover:-translate-y-1 transition-transform relative pb-8" v-for="(anime , index) in getdata" :key="index">
              <img :src="anime.images.jpg.large_image_url" alt="imageanime" class="rounded object-cover object-center w-[225px] h-[319px] mx-auto my-2" >
              <p class="font-medium px-3 pb-3">{{ anime.title }}</p>
            <button @click.prevent="detailanime(anime.mal_id)" class="absolute bottom-0 left-0 w-full bg-[#82CCDD] font-medium py-2 rounded-b hover:bg-[#45b2ca] transition-all">Read More</button>
          </div>
      </div>
      </template>
      <p v-if="notfound" class="text-white text-3xl font-medium text-center">Anime Not Found</p>
  </div>

    <div class="fixed w-[95%] md:w-[700px] h-[460px] bg-slate-400 z-30 top-0 bottom-0 left-0 right-0 m-auto rounded-xl overflow-y-scroll overflow-x-hidden" :class="detail ? 'block' : 'hidden'">

        <div class="flex font-medium">
            <p class="py-2 px-3 rounded-tl-xl transition-all" @click="ingfoanime = true" :class="ingfoanime ? 'bg-slate-300' : ''">Info Anime</p>
            <p class="py-2 px-3 transition-all" @click="getdatachar(datadetail.mal_id)" :class="ingfoanime ? '' : 'bg-slate-300'">Character</p>
        </div>
        
        <!-- Ingo Anime Start -->
        <div :class="ingfoanime ? 'block' : 'hidden'">
        <template v-if="loadingdetail">
            <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="margin: auto; background: none; display: block; shape-rendering: auto;" width="100px" height="100px" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid"><rect x="17.5" y="30" width="15" height="40" fill="#93dbe9"><animate attributeName="y" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="18;30;30" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.2s"></animate><animate attributeName="height" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="64;40;40" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.2s"></animate></rect><rect x="42.5" y="30" width="15" height="40" fill="#689cc5"><animate attributeName="y" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="20.999999999999996;30;30" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.1s"></animate><animate attributeName="height" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="58.00000000000001;40;40" keySplines="0 0.5 0.5 1;0 0.5 0.5 1" begin="-0.1s"></animate></rect><rect x="67.5" y="30" width="15" height="40" fill="#5e6fa3"><animate attributeName="y" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="20.999999999999996;30;30" keySplines="0 0.5 0.5 1;0 0.5 0.5 1"></animate><animate attributeName="height" repeatCount="indefinite" dur="1s" calcMode="spline" keyTimes="0;0.5;1" values="58.00000000000001;40;40" keySplines="0 0.5 0.5 1;0 0.5 0.5 1"></animate></rect></svg>
        </template>
        <template v-else>           
        <div class="flex flex-col md:flex-row">
            <img v-if="detail" :src="datadetail.images.jpg.image_url" alt="imgdetail" class="rounded-md m-3 w-[200px] md:w-max h-[300px]">

            <div class="pt-5 pr-2 md:pr-5 pl-3 md:pl-2" v-if="detail">
                <p class="font-['Poppins']">Title : {{ datadetail.title }}</p>
                <p class="font-['Poppins'] mt-1">Title Japanesse : {{ datadetail.title_japanese }}</p>
                <div class="flex items-center flex-wrap">
                <p class="font-['Poppins']">Genres : </p>
                <p class="py-2 px-3 rounded-md bg-slate-300 w-max font-medium ml-2 mt-1" v-for="(genre , index) in datadetail.genres" :key="index">{{ genre.name }}</p>
                </div>
                <p class="font-['Poppins'] mt-1">Realese : {{ datadetail.aired.string }}</p>
                <p class="font-['Poppins']">Score : {{ datadetail.score }}</p>
                <p class="font-['Poppins']">Type : {{ datadetail.type }}</p>
                <p class="font-['Poppins']">Episodes : {{ datadetail.episodes }}</p>

                <iframe class="my-3 rounded-md w-[350px] h-[200px] md:w-[420px] md:h-[240px]"
                :src="datadetail.trailer.embed_url">
                </iframe>
            </div>
        </div>
            <p class="font-['Poppins'] px-3 py-4 bg-slate-300">{{ datadetail.synopsis }}</p>
        </template>
        </div>
        <!-- Ingo Anime End -->

        <!-- character start -->
            <div :class="ingfoanime ? 'hidden' : 'block'">
                <template v-if="loadingchar">
                    <div class="flex flex-wrap justify-evenly my-3">
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    <div class="w-[150px] h-[233px] animate-pulse bg-slate-300 mb-3 rounded-md"></div>
                    </div>
                </template>
                <template v-else>                    
                <div class="flex flex-wrap justify-evenly my-3">
                    <template v-for="char in datachar">
                        <div v-if="datachar" class="mb-3 relative group">
                            <img :src="char.character.images.jpg.image_url" alt="charimg" class="w-[150px] rounded">

                            <div class="absolute flex justify-center flex-col items-center bg-[#00000073] top-0 w-full h-full text-white opacity-0 transition-all duration-500 group-hover:opacity-100">
                            <p class="font-medium text-center px-2">{{ char.character.name }}</p>
                            <p>{{ char.role }}</p>
                            </div>
                        </div>
                    </template>
                </div>
                </template>
            </div>
        <!-- character end -->
    </div>

  <div class="fixed top-0 left-0 w-screen h-screen bg-[#0d101194]" :class="detail ? 'block' : 'hidden'" @click="cleardata"></div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            cari: '',
            getdata: [],
            datadetail: [],
            datachar: [],
            detail: false,
            loading: false,
            loadingdetail: false,
            loadingchar: false,
            ingfoanime: false,
            notfound: false
        }
    },
    methods: {
        async carianime(){
            this.notfound = false
            this.loading = true
            try {
                let { data } = await axios.get(`https://api.jikan.moe/v4/anime?order_by=title&q=${this.cari}`)
                this.getdata = data.data
                this.loading = false
                if (data.data.length == 0) {
                    this.notfound = true
                }else{
                    this.notfound = false
                }
            } catch (error) {
                this.loading = true
            }
        },
        async detailanime(id){
            this.loadingdetail = true
            try {
                this.detail = true
                this.ingfoanime = true
                let { data } = await axios.get(`https://api.jikan.moe/v4/anime/${id}`)
                this.datadetail = data.data
                this.loadingdetail = false
            } catch (error) {
                this.loadingdetail = true
            }
        },
        cleardata(){
            this.detail = false
            this.datadetail = []
            this.datachar = []
        },
        async getdatachar(id){
            this.ingfoanime = false
            this.loadingchar = true

            try {                
                let { data } = await axios.get(`https://api.jikan.moe/v4/anime/${id}/characters`)
                this.datachar = data.data
                this.loadingchar = false 
            } catch (error) {
                this.loadingchar = true
            }
        }
    },
}
</script>