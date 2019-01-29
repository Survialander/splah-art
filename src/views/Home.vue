<template>
    <div class="home">
        <ul>
            <li>
                <input type="checkbox" id="mage" value="Mage" v-model="championTag">
                <label for="mage">Mago</label>
            </li>
            <li>
                <input type="checkbox" id="assassin" value="Assassin" v-model="championTag">
                <label for="assassin">Assassin</label>
            </li>
            <li>
                <input type="checkbox" id="tank" value="Tank" v-model="championTag">
                <label for="tank">Tank</label>
            </li>
            <li>
                <input type="checkbox" id="support" value="Support" v-model="championTag">
                <label for="support">Support</label>
            </li>
            <li>
                <input type="checkbox" id="marksman" value="Marksman" v-model="championTag">
                <label for="marksman">Atirador</label>
            </li>
             <li>
                <input type="checkbox" id="fighter" value="Fighter" v-model="championTag">
                <label for="fighter">Lutador</label>
            </li>
        </ul>
        <label for="">Name</label>
        <input type="text" v-model="championName">
        <transition-group name="list" tag="div">
            <router-link class="list-item" :to="`/splash/${key}`" v-for="(champion, key) in championsFiltreds" :key="champion.key">
                <img :src="getIconSquareChamp(key)" alt="">
            </router-link>
        </transition-group>
    </div>
</template>

<script>
    Object.filter = (obj, predicate) =>
        Object.keys(obj)
        .filter(key => predicate(obj[key]))
        .reduce((res, key) => (res[key] = obj[key], res), {});

    export default {
        name: 'home',
        data: () => ({
            champions: [],
            championTag: [],
            championName: ''
        }),
        mounted() {
            this.getChampions();
        },
        computed: {
            championsFiltreds() {
                let filtredsByTags = Object.filter(this.champions, e => e.tags.some(e => this.championTag.some(i => i == e)))
                
                return Object.filter(filtredsByTags, e => {
                    let regex = new RegExp(`${this.championName}(\\w+)?`)
                    return regex.test(e.name)
                }); 
            },
        },
        methods: {
            getChampions() {
                this.axios.get('http://ddragon.leagueoflegends.com/cdn/9.2.1/data/pt_BR/champion.json')
                    .then(res => res.data.data)
                    .then(res => {
                        this.champions = res
                    })
            },
            getIconSquareChamp(champName) {
                return `http://ddragon.leagueoflegends.com/cdn/9.2.1/img/champion/${champName}.png`
            }
        }
    }
</script>
<style>
.list-item {
  transition: all 1s;
  display: inline-flex;
  justify-content: center;
  margin-right: 10px;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.list-leave-active {
  position: absolute;
}
</style>
