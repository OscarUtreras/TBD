<template>
    <v-app dark id="app-movies">
        <v-container grid-list-xs>
            <v-layout row wrap>
                <v-flex lg2 offset-lg1 class="movie-box" v-for="film in films" :key="film.id">
                    <div v-if="film.img !== null">
                        <v-card width="300px" class="movie-card"  hover :to="{name: 'film', params:{ id: film.id }}">
                            <v-card-media :src="img+film.img" height="450">
                            </v-card-media>
                            <v-card-title primary-title class="title-card">
                                <div>
                                    <h3 class="headline mb-0">{{ film.nombre}}</h3>
                                </div>
                            </v-card-title>
                        </v-card>
                    </div>
                </v-flex>
            </v-layout>
        </v-container>
    </v-app>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'app-movies',
        data: function () {
            return {
                films: null,
                img: 'https://image.tmdb.org/t/p/w500',
                api_key: "/images?api_key=946fc3c5365c912765fffe16866d65ed",
                url: 'http://167.99.155.164:8080'
            }
        },
        mounted(){
            this.getFilms();
        },
        methods: {
            getFilms() {
                axios.get(this.url+'/tweetwood_back-0.0.1-SNAPSHOT/peliculas/disponibles')
                    .then((films)=>{this.films=films.data;})
                    .catch((err) => console.error(err))
                    .then(() => {
                        for(let i = 0; i < this.films.length; i++){
                            axios.get('https://api.themoviedb.org/3/movie/' + this.films[i].idApi + this.api_key)
                                .then((movie) => {
                                    if(movie.data.posters[0].file_path !== undefined)
                                        this.films[i].img = movie.data.posters[0].file_path;
                                    else
                                        this.films[i].img = null;
                                })
                                .catch((err) => console.error(err));
                        }
                    })
                    .catch((err) => console.error(err));
            }
        }
    }
</script>

<style scoped>
    /*.movie-card {
        margin: 5% 20%;
    }*/
    /*.movie-card-actions {
        margin: 1% 1%;
        padding: 0px 90px;
    }*/

    .title-card {
        background-color: #212121;
    }
</style>
