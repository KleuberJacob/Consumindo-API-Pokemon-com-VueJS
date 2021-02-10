<template>
    <div id="pokemon">    
        <div class="card">
            <div class="card-image">
                <figure>
                <img :src="currentImg" alt="Placeholder image">
                </figure>
            </div>
            <div class="card-content">
                <div class="media">                
                <div class="media-content">
                    <p class="title is-4">{{ num }} - {{ name | upperCase }}</p>
                    <p class="subtitle is-6">{{ pokemon.type }}</p>
                </div>
                </div>
                <div class="content">
                    <button class="button is-medium is-fullwidth" @click="mudarSprite">Mudar Sprite</button>
                </div>
            </div>
        </div>
    </div>  
</template>

<script>
import axios from 'axios'

export default {
    props: {
        num: Number,
        name: String,
        url: String
    },
    data(){
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                front: '',
                back: ''
            }
        }
    },
    created: function(){//Funcao para requisicao via axios utilizando url personalizada de cada elemento do array
        axios.get(this.url).then(res => {
            this.pokemon.type = res.data.types[0].type.name //Armazenando os dados a serem utilizados dentro de variaveis ligadas ao objeto(pokemon) criado           
            this.pokemon.front = res.data.sprites.front_default
            this.pokemon.back = res.data.sprites.back_default
            this.currentImg = this.pokemon.front            
        })
    },
    methods: {
        mudarSprite: function(){
            if(this.isFront){
                this.isFront = false
                this.currentImg = this.pokemon.back
            }else {
                this.isFront = true
                this.currentImg = this.pokemon.front
            }
        }
    },
    filters: {
        upperCase: function(value){//Funcao/filtro para colocar primeira letra para maiuscula que recebe como parametro um nome
            let newName = value[0].toUpperCase() + value.slice(1)//Pegando a primeira letra pelo array e descartando a primeira letra original que e maiuscula
            return newName
        }
    }
}
</script>

<style>
    #pokemon {
        margin-top: 5%;
    }
</style>