<template>
    <div id="top-games">
        <p id="top-games-header" class="text-info ml-5 pt-3">| Top games</p>
        <div class="container-fluid">
            <div class="row ml-5">
                <template v-for="game in splicedGames">
                    <div class="col-lg-3 col-md-6 col-sm-6 col-12 mb-4" :key="game.game._id">
                        <div class="card border-0 h-100 w-75">
                            <router-link :to="{name: 'Game', params:{gameName: game.game.name}}">
                                <img :src="game.game.box.large" class="card-img-top img-fluid" alt="game-thumbnail" />
                            </router-link>
                            <div class="card-body">
                            <div class="d-sm-flex h-45">
                                <h5 class="card-title text-white pt-1 text-truncate"><span id="green-line" class="text-info pr-1">|</span>{{ game.game.name }}</h5>
                            </div>

                            <div class="d-sm-flex">
                                <span id="add-to-fav" class="text-muted">Add to favourites</span>
                                <i class="far fa-star text-white pl-1 pt-1" @click="addToFav(game.game.name, game.game._id, game.game.box.large, type)"></i>
                            </div>
                            </div>
                        </div>
                    </div>
                </template>
            </div>
        </div>
        <div class="row justify-content-center mt-3">
            <button
                class="btn btn-info mb-5 rounded-pill px-5 shadow m-auto"
                @click="gamesShown += 4" v-if="gamesShown < topGames.length"
            >Load More</button>
        </div>
    </div>
</template>

<script>
    import axios from "axios";
    import Button from "@/components/Button";

    export default {
    name: "TopGames",
    components: {
        Button
    },
    data() {
        return {
            topGames: [],
            gamesShown: 4,
            type: "games"
        }
    },
    computed: {
        splicedGames() {
            const splicedGames = this.topGames.slice(0, this.gamesShown);
            return splicedGames;
        }
    },
    methods: {
        async getTopGames() {
            try {
                const response = await axios.get(
                "https://api.twitch.tv/kraken/games/top",
                    {
                        headers: {
                        "Client-ID": "2c6c0j18xxty4mx9e43pn1p5s210u9",
                        Accept: "application/vnd.twitchtv.v5+json"
                        }
                    }
                );
                this.topGames = response.data.top
            } catch (error) {
                console.log(error);
            }
        },
        addToFav(name, id, img, type){
            this.$store.dispatch('addToFavourites', {
                name: name,
                id: id,
                img: img,
                type: this.type
            })
        }
    },
    mounted() {
        this.getTopGames();
    }
    }
</script>

<style>
    #top-games-header {
        font-size: 23px;
    }
    .card {
        width: 18rem;
    }
    .card-body {
        background-color: #1a202c;
    }
    #top-games {
        background-color: #2d3748;
    }
    #green-line {
        font-size: 20px;
    }
    .card-title {
        font-size: 15px;
    }
</style>