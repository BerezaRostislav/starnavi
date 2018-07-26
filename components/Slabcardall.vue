<template>
    <v-container grid-list-md text-xs-center fluid>
        <v-layout align-start justify-center fill-height row wrap>
                <v-card class="slabcard" >
                    <v-layout row wrap>
                    <v-flex xs4 sm4 md4 >
                        <div class="slabfrontimage" :style="{backgroundImage: 'url('+ slabfrontimage + ')'}"></div>
                        <no-ssr>
                        <star-rating v-model="rating" :read-only="true" :show-rating="false" :glow="16" :rounded-corners="true" :star-size="30" :star-points="[23,2, 14,17, 0,19, 10,34, 7,50, 23,43, 38,50, 36,34, 46,19, 31,17]"></star-rating>
                        </no-ssr>
                    </v-flex>
                    <v-flex xs8 sm8 md8 >
                        <h1>{{ full_address }}</h1>
                        <span>{{ description }} </span>
                        <div style="text-align: center;">
                          <span style="margin-right: 20px;"><v-icon>home</v-icon>Кол-во комнат - {{ rooms }} </span>
                          <span style="margin-left: 20px;"><v-icon>event</v-icon>{{ public_date }} </span>
                        </div>
                        <v-btn absolute right bottom :to="'/' + id" style="margin-right: 10px; margin-bottom: 40px; color:red;"> Купить за - {{ price }}$</v-btn>
                    </v-flex>
                    </v-layout>
                </v-card>
        </v-layout>
    </v-container>
</template>

<script>
import StarRating from 'vue-star-rating'
export default {
    components: {
        StarRating
    },
    props: ['slabfrontimage', 'price', 'id', 'rooms','rating','full_address','description','public_date']
}
</script>

<style scoped>
a {
    text-decoration: none;
    color: black;
}

.slabcard{
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
    /* height: 500px; */
    min-height: 400px;
    width: 90%;
    padding-top: 0px;
    padding: 8px;
    border: 1px solid #ccc;
    box-shadow: 0 2px 2px #aaa;
    margin-top: 0px;
    margin: 10px;

}

.slabfrontimage {
    background-position: center;
    background-size: cover;
    width: 90%;
    height: 90%;
}

</style>
