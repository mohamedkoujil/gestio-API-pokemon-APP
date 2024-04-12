<template>
    <div class="pokemonCard flexColumn center alignself-left verdanaFont" v-show="ok" id="datos">
        <img v-if="favorite" class="margin05 alignself-right fav-img"
            @click="$emit('togglefav')" src="../assets/fav.png">
        <img v-else class="alignself-right fav-img" @click="$emit('togglefav')" src="../assets/no-fav.png">
        <img class="img-pokemon" :src="img">
        <div class="title">{{ pokemonData.name }}</div>
        <div class="flex flexStart flexColumn" v-show="info" id="masInfo">
            <div v-for="(type, index) in pokemonData.types" :key="index">
                <div class="margin025">{{ type.type.name }}</div>
            </div>
            <div v-for="(ability, index) in pokemonData.abilities" :key="index">
                <div class="margin025">{{ ability.ability.name }}</div>
            </div>
            <div v-for="(stat, index) in pokemonData.stats" :key="index">
                <div class="margin025">{{ stat.stat.name }}: {{ stat.base_stat }}</div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    // Establece un nombre al componente. De lo contrario aparece como "Anónimo" en DevTools.
    name: "PokemonCard",
    // Función que devuelve un  con las variables del componente Vue.
    data() {
        return {};
    },
    props: {
        pokemon_data: String, 
        favorite: Boolean, 
        img: String, 
        info: Boolean, 
        ok: Boolean
    },
    // Lista de funciones (métodos) disponibles en el componente Vue.
    methods: {
        toggleFav() {
            this.$emit('togglefav');
        },
    },
};
</script>
<style>
/* Estilos CSS */
.pokemonCard {
    width: 9em;
    background-color: rgb(255, 219, 88); /* Amarillo claro */
    border: rgb(112, 128, 250) 5px solid; /* Azul claro */
    border-radius: 10px;
    margin: 0.5em;
    padding: 0.5em;
}

</style>