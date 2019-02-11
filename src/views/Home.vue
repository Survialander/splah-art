<template>
    <div class="home">
        <ul>
            <li class="lastChild">
                <label for="champName">Nome:</label>
                <input id="champName" class="nameInput" placeholder="Nome do Campeão" type="text" v-model="championName">
            </li>
                
            
            <img class="logo" :src="require('../assets/img/jungle_icon.png')"/>
            <img class="logo" :src="require('../assets/img/middle_icon.png')"/>
            <img class="logo" :src="require('../assets/img/marksman_icon.png')"/>
            <img class="logo" :src="require('../assets/img/support_icon.png')"/>
            <img class="logo" :src="require('../assets/img/top_icon.png')"/>
          

            <li>
                <input type="checkbox" id="mage" value="Mage Assassin" v-model="championTag">
                <label for="mage">All</label>
            </li>
            <li>
                <input type="checkbox" id="mage" value="Mage" v-model="championTag">
                <label for="mage">Mage</label>
            </li>
            <li>
                <input type="checkbox" id="assassin" value="Assassin" v-model="championTag">
                <label for="assassin">Assasin</label>
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
            championName: '',
        }),
    
        mounted() {
            this.getChampions();
        },
        computed: {
            championsFiltreds() {
                let filtredsByTags = Object.filter(this.champions, e => 
                    e.tags.some(e => this.championTag.some(i => i == e)))
                
                return Object.filter(filtredsByTags, e => {
                    let regex = new RegExp(`${this.championName.toLowerCase()}(\\w+)?`)
                    return regex.test(e.name.toLowerCase())
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
div{
    font-family: Arial, Helvetica, sans-serif;  
}
ul{
    list-style: none;
}
li{
    float: right;
    padding: 10px;
}
.list-item {
  transition: all 1s;
  display: inline-flex;
  justify-content: center;
  padding: 2px;
  margin-top: 10px;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.list-leave-active {
  position: absolute;   
}
.logo{
    width: 32px;
    height: 32px; 
    border-radius: 4px; 
    background-color: black;
    transition: 0.5s all;    
}
.logo:hover{
    margin-right: 4px;
    margin-left: 4px;
    width: 34px;
    height: 34px;
    transition-delay: 0.5s all;
}
.nameInput{
    transition: 0.5s all;
    border: solid 1px gray;
    border-radius: 3px; 
    width: 120px;
    padding: 3px;
}
.nameInput:focus{
    width: 150px;
    transition: 0.5s all;
}
.lastChild{
    float: left;
}
img{
    width: 64px;
    height: 64px;
}
</style>
