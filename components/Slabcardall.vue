<template>
    <v-container fluid>
        <v-layout row wrap>
            <v-flex xs6 offset-xs1>
                <v-card class="slabcard" >

                <div class="slabfrontimage" :style="{backgroundImage: 'url('+ slabfrontimage + ')'}"></div>
                <span>{{ full_address }}</span>
                <span>{{ description }} </span>
                <span>{{ public_date }} </span>

                <div style="text-align: center;">    
                <span style="margin-left: 10px;">Кол-во комнат - {{ rooms }}  </span>
                <v-btn :to="'/' + id" style="margin-right: 10px;color:red;"> Купить за - {{ price }}$</v-btn>
                </div>
                    <div>
                        <no-ssr>
                             <star-rating v-model="rating" :read-only="true" :show-rating="false" :glow="16" :rounded-corners="true" :star-points="[23,2, 14,17, 0,19, 10,34, 7,50, 23,43, 38,50, 36,34, 46,19, 31,17]"></star-rating>
                        </no-ssr>
                    </div>
                </v-card>
            </v-flex>
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
    height: 500px;
    width: 650px;
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
    width: 265px;
    height: 460px;
}

</style>
