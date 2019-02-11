<template>
<div class="about">
      <span> {{ champName }} </span>
    <Flickity v-if="Object.keys(skins).length > 0" ref="Flickity" :options="flickityOptions">
        <div v-for="skin in skins" :key="skin.num" class="carousel-cell">                
            <img class="img"  :src="getSplashArtMain(skin.num)"> 
        </div>
    </Flickity>
</div>
</template>

<script>
import Flickity from 'vue-flickity'

    export default {
        components: {
            Flickity
        },
        data() {
            return {
                champion: {},
                champName: this.$route.params.champ,
                flickityOptions: {
                                initialIndex: 0,
                                prevNextButtons: true,
                                pageDots: false,
                                wrapAround: true,
                                freeScroll: false
                                // any options from Flickity can be used
                            }
            }
        },
        mounted() {
            this.getChampion()
        },
        computed: {
            skins() {
                return Object.values(this.champion).length && Object.values(this.champion)[0].skins || []
            }
        },
        methods: {
            getChampion(champName) {
                this.axios.get(`http://ddragon.leagueoflegends.com/cdn/9.2.1/data/pt_BR/champion/${this.champName}.json`)
                    .then(res => res.data.data)
                    .then(res => {
                        this.champion = res
                    })
            },
            getSplashArtMain(idSplash) {
                return `http://ddragon.leagueoflegends.com/cdn/img/champion/splash/${this.champName}_${idSplash}.jpg`
            },   
        }
    }
</script>
<style>
.img {
    width: 600px;
    height: 400px;
    border-radius: 3px;
}
span{
    font-family:Arial, Helvetica, sans-serif;
    font-size: 30px;
    font-style: italic;
    margin-bottom: 5px;
}
</style>
