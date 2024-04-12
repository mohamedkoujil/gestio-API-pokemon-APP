<template>
  <div class="form-container center flex">
      <form class="center">
          <input id="inputUsr" type="text" placeholder="Ej. Charmander" required>
      </form>
      <img class="serach alignself-center" src="assets/lupa.png" @click="clickLupa">
      <button v-if="!masInfo" class="buttonInfo" @click="masInfo = !masInfo">Mas info</button>
      <button v-else class="buttonInfo" @click="masInfo = !masInfo" style="background-color: rgb(186, 224, 255); color: black;">Mas info</button>
  </div>

  <PokemonCard :pokemon_data="pokemonData" :favorite="isInFavorites(pokemonData)" :img="imgLink(pokemonData)"
    :info="masInfo" :ok="ok" @togglefav="toggleFavorite(pokemonData)"></PokemonCard>

  <div v-if="pokemonNoEncontrado">No se ha encontrado el pokemon</div>

  <!--
  <div class="flex wrap">
      <div v-for="pokemon in allPokemon" :key="pokemon">
          <pokemon-card :pokemon_data="pokemon" :favorite="isInFavorites(pokemon)" :img="imgLink(pokemon)" :info="masInfo" :ok="true" @togglefav="toggleFavorite(pokemon)"></pokemon-card>
      </div>
  </div>
  -->

  <h2 class="alignself-left">Favoritos</h2>
  <div class="alignself-left" v-if="favoritos.length == 0">No tienes favoritos</div>
  <div v-else class="flex wrap alignself-left">
    <div class="flex" v-for="fav in favoritos" :key="fav">
      <pokemon-card :pokemon_data="fav" :favorite="isInFavorites(fav)" :img="imgLink(fav)" :info="masInfo" :ok="true"
        @togglefav="toggleFavorite(fav)"></pokemon-card>
    </div>
  </div>
</template>

<script>
import PokemonCard from './components/pokemon-card.vue'
const apiLink = 'https://pokeapi.co/api/v2/'
export default {
  name: 'App',
  data() {
    return {
      pokemon: '',
      ok: false,
      pokemonData: {},
      showImg: false,
      pokemonNoEncontrado: false,
      favoritos: [],
      masInfo: false,
      allPokemon: []
    }
  },

  components: {
    PokemonCard
  },

  methods: {
    verPokemon() {
      if (this.pokemon == '') {
        return;
      }
      console.log(this.pokemon);
      this.conectaApi("pokemon/" + this.pokemon.toLowerCase());
    },

    conectaApi(pokemon) {
      fetch(apiLink + pokemon)
        .then((res) => {
          if (res.ok) {
            return res.json();
          } else {
            this.ok = false;
            this.pokemonNoEncontrado = true;
            throw new Error('Error en la llamada a la API');
          }
        })
        .then((js) => {
          this.pokemonData = js;
          console.log(this.pokemonData);
          this.showImg = true;
          this.ok = true;
          this.pokemonNoEncontrado = false;
          console.log(this.pokemonData+"ASd");
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },

    imgLink(pokemon) {
      const id = pokemon.id;
      switch (true) {
        case id < 10:
          return "https://assets.pokemon.com/assets/cms2/img/pokedex/full/00" + id + ".png";
        case id < 100:
          return "https://assets.pokemon.com/assets/cms2/img/pokedex/full/0" + id + ".png";
        default:
          return "https://assets.pokemon.com/assets/cms2/img/pokedex/full/" + id + ".png";
      }
    },

    setPokemonName(pokemon) {
      this.pokemon = pokemon;
    },

    toggleFavorite(pokemon) {
      if (this.isInFavorites(pokemon)) {
        this.removeFav(pokemon);
      } else {
        this.addFav();
      }
    },

    addFav() {
      let pokemonName = this.pokemonData.name;
      if (!this.favoritos.some(fav => fav.name == pokemonName)) {
        this.favoritos.push(this.pokemonData);
        localStorage.setItem('favorites', JSON.stringify(this.favoritos));
      }
    },

    removeFav(pokemon) {
      this.favoritos = this.favoritos.filter(fav => fav.name != pokemon.name);
      localStorage.setItem('favorites', JSON.stringify(this.favoritos));
    },

    loadFavorites() {
      const favorites = localStorage.getItem('favorites');
      if (favorites) {
        this.favoritos = JSON.parse(favorites);
      }
    },

    isInFavorites(pokemon) {
      return this.favoritos.some(fav => fav.name == pokemon.name);
    },

    clickLupa() {
      this.pokemon = document.getElementById('inputUsr').value;
      this.verPokemon();
    },

    loadAllPokemon() {
      console.log(apiLink + "pokemon?limit=100000&offset=0");
      fetch(apiLink + "pokemon?limit=100000&offset=0")
        .then((res) => {
          if (res.ok) {
            return res.json();
          } else {
            this.ok = false;
            this.pokemonNoEncontrado = true;
            throw new Error('Error en la llamada a la API');
          }
        })
        .then((js) => {
          this.allPokemon = js.results;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    }
  },
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
