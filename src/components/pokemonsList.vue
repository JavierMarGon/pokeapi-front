<template>
    <div class="fondo">
        <h1>POCHODEX</h1>
        <div>
            <input type="text" placeholder="Nombre del pokemón Ej. ivysaur" style="padding: 10px; width: 300px;">
            <button style="padding: 10px;">Buscar</button>
        </div>
        <div class="caja_pokemon">
            <div v-for="(pokemon,index) in pokemonImg":key="index" class="pokemon">
                <img :src="pokemon" :alt="pokemonName[index]">
                <p>{{ pokemonName[index] }}</p>
            </div>
        </div>
        
    </div>
</template>
<script setup >
    import{ref,onBeforeMount} from 'vue';
    let pokemonImg=ref([]);
    let pokemonName=ref([]);

    function getPokemonData(ind){
        fetch("https://pokeapi.co/api/v2/pokemon-form/"+ind+"/").then(response=>response.json()).then(data=>{pokemonImg.value.push(data.sprites.front_default)});
    }
    
        
    onBeforeMount( async() => {
        fetch("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(response=>response.json()).then(data=>
            {data.results.forEach((pokemon, index) => {
                getPokemonData(index + 1)
                pokemonName.value.push(pokemon.name);
            })
        });
    });
    console.log(pokemonImg);
    console.log(pokemonName);
</script>
<style scoped>
.fondo {
    background-color: blue;
    color: white;
    text-align: center;
    font-family: Arial, sans-serif;
    

    h1 {
        color: yellow;
    }
    
    .caja_pokemon {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        margin-top: 20px;

        .pokemon{
            margin: 10px; 
            text-align: center;

            img{
                width: 100px; height: 100px;
            }

            p{
                color: white;
            }
        }
    }
}


</style>