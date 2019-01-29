<template>
    <div class="about">
        <div class="img" v-for="skin in skins" :key="skin.num">
            <img :src="getSplashArtMain(skin.num)">
        </div>
    </div>
</template>

<script>
    export default {
        data: self => ({
            champion: {},
            champName: self.$route.params.champ
        }),
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
            }
        }
    }
</script>
<style>
.img {
    width: 500px;
    height: 300px;
    display: block;
}
.img img {
    object-fit: cover;
    width: 100%;
    height: 100%;
}
</style>
