
<template>
    <div class="main-block">
        <h3>Your Fav</h3>
        <div v-for="genre in favGenreArray" :key="genre.id" class="main-block">


            <button id="remove-button" @click="removeGenre(genre.id)">Remove</button>
            {{ genre.name }}
        </div>

        <h3>Add to your Fav</h3>
        <div v-for="genre in unfavGenreArray" :key="genre.id" class="main-block">


            <button id="add-button" @click="addGenre(genre.id)">Add</button>
            {{ genre.name }}
        </div>
    </div>
</template> 
  
<script>
import { computed } from 'vue';
import AccountService from '../services/AccountService';


export default {
    data() {
        return {
            storeGenres: [],
        }
    },

    props: {
        genres: {
            type: Array,
            required: true
        },
        AccountService: {
            type: Object,
        },

    },
    created() {

        this.storeGenres = this.$store.commit("UPDATE_GENRES");


    },
    methods: {
        convertToGenreString(genre) {


            for (let i = 0; i < genre.length; i++) {

                if (genre[i].id) {
                    return genre[i].name
                }
            }
            return "??"

        },
        addGenre(id) {

            AccountService.addGenre(id);
            // eslint-disable-next-line vue/no-mutating-props
            this.genres.push(id);


        },

        removeGenre(id) {

            AccountService.removeGenre(id);
            // eslint-disable-next-line vue/no-mutating-props
            this.genres.splice(this.genres.indexOf(id), 1);

        },


    },
    computed: {
        favGenreArray: function () {
            if (this.genres == undefined) {
                return []
            }
            if (this.genres == null) {
                return []
            }

            let out = [];

            // if (this.genres == null) {


            //     this.genres = [];
            // }
            for (let i = 0; i < this.$store.state.genres.length; i++) {
                if (this.genres.includes(this.$store.state.genres[i].id)) {
                    out.push(this.$store.state.genres[i])
                }
            }
            return out
        },
        unfavGenreArray: function () {
            let gEmpty = false;
            if (this.genres == undefined) {


                gEmpty = true;
            }
            if (this.genres == null) {

                gEmpty = true;
            }

            // if (this.genres == undefined) {
            //     return []
            // }
            // if (this.genres == null) {
            //     return []
            // }



            let out = [];

            for (let i = 0; i < this.$store.state.genres.length; i++) {
                if (gEmpty || !this.genres.includes(this.$store.state.genres[i].id)) {
                    out.push(this.$store.state.genres[i])
                }
            }

            return out
        }
    }

}

</script>
  
<style scoped>
#remove-button {
    margin-left: 5px;
    background-color: #890304;
    color: #fff0cb;
    border: 1px solid #890304;
    border-radius: 4px;
    font-family: 'league spartan';
    margin-bottom: 5px;

}

#add-button {
    margin-left: 5px;
    background-color: #890304;
    color: #fff0cb;
    border: 1px solid #890304;
    border-radius: 4px;
    font-family: 'league spartan';
    margin-bottom: 5px;
}

/* .movie-img {
    max-width: 14%;
    max-height: 14%;
    margin: 5px;
  }
  
  .main-block {
    display: flex;
    flex-direction: row;
    border: .001rem solid #890304;
    width: 70%;
    margin-left: 5%;
  }
  
  .column {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    justify-content: start;
  }
  
  .movie-title {
    font-weight: bold;
    color: #890304;
    font-size: 1.5rem;
  
  }
  
  .release-genres {
    color: gray;
    margin-bottom: .8rem;
    font-size: .8REM;
  }
  
  .description {
    color: #890304;
    margin-bottom: 5px;
    font-size: 1rem;
    width: 70%;
  }
  
  .rating {
    color: #890304;
    font-size: 13px;
  } */
</style>
  
  
  
  