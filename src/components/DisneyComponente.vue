<template>
    <div class="page">
        <div class="buscar">            
            <img alt="Disney" src="https://static-mh.content.disney.io/matterhorn/assets/goc/disney_logo_dark@2x-45d70f7dd57b.png" class="disney-img">            
            <input 
                type="text"
                placeholder="Buscar personagem"
                v-model="search"
                @input="onSearchChange"
            >
        </div>
        <ul class="container">
            <li v-for="(character, key) in characters" :key="key" class="card">
                <div class="content">
                    <a :href="character.imageUrl" target="_blank">
                        <img :src="character.imageUrl">
                    </a>
                    <h2>{{ character.name }}</h2>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
    import api from '../services/api.js'

    export default {
        name: "DisneyComponente",
        components: {
            api
        },
        data() {
            return {
                search: "",
                characters: [],
                timeout: null
            }
        },
        mounted(){
            this.buscar()
        },
        methods: {
            buscar() {                
                let url = "&name=" + this.search
                api.get(url)
                    .then(ret => {
                        if (Array.isArray(ret.data.data)) {
                            this.characters = ret.data.data;
                        } else {
                            this.characters = [ret.data.data];
                        }
                        console.log(this.characters);
                    });
            },
            onSearchChange() {
                this.characters = [];
                clearTimeout(this.timeout);                
                this.timeout = setTimeout(() => {
                    this.buscar();
                }, 300);
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Poppins:200,300,400,500,600,700,800,900&display=swap');

    :root {
        --height-buscar: 250;
    }

    .buscar {
        position: relative;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        top: 50px;
        color: #fff;
        width: 100%;
        margin-bottom: 50px;
        justify-content: center;
        align-items: center;
    }
    .buscar img {
        filter: invert();
        margin-bottom: 15px;
    }
    .buscar input {
        border: 1px solid #fff;
        border-radius: 5px;
        height: 25px;
        padding-left: 10px;        
        color: gray;
        width: 60%;
    }
    .page
    {
        position: absolute;
        display: flex;
        flex-direction: column;
        min-height: 100%;
        min-width: 100%;
        justify-content: center;
        align-items: center;
        background: linear-gradient(31deg, rgba(131,58,180,1) 0%, rgba(253,29,29,1) 50%, rgba(182,31,246,1) 100%);     
    }
    .container
    {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        max-width: 1200px;
        flex-wrap: wrap;
    }
    .container .card
    {
        position: relative;
        width: 280px;
        height: 400px;
        margin: 30px;
        box-shadow: 20px 20px 50px rgb(0, 0, 0, 0.5);
        border-radius: 15px;
        background: rgba(255, 255, 255, 0.1);
        overflow: hidden;
        display: flex;
        justify-content: center;
        text-align: center;
        border-top: 1px solid rgba(255, 255, 255, 0.5);
        border-left: 1px solid rgba(255, 255, 255, 0.5);
        backdrop-filter: blur(5px);
    }
    .container .card .content
    {
        position: relative;
        padding: 10px;
        width: 100%;
        text-align: center;
        transition: 0.5s;
    }
    .container .card .content h2
    {
        font-size: 1.3rem;
        color: #fff;
    }
    .container .card .content a
    {
        position: relative;
        width: 100%;
        height: 80%;        
    }
    .container .card .content a img
    {
        display: flex;
        position: relative;
        width: 100%;
        height: 85%;
        object-fit: contain;
    }
    @media (max-width: 700px) {
        .page::before
        {
            clip-path: circle(5% at right 70%);
        }
        .page::after
        {
            clip-path: circle(1.2% at 10% 1%);
            z-index: 1;
        }
        .buscar {
            margin-bottom: 65px;
        }
        .container .card {
            margin: 15px;
        }
    }
</style>
