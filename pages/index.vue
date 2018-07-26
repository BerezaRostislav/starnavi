<template>
  <v-container column>
    <v-layout align-center justify-center>
      <div class="text-xs-center">
        <logo/>
      </div>
    </v-layout>
    <v-layout>
    <v-flex xs12 sm3 md3 lg3 fluid>
      <v-card style="width:95%; justify-center">
        <v-card-text>ВАЛЮТА</v-card-text>      
        <v-btn-toggle v-model="text">
          <v-btn v-on:click="UAH" flat value="UAH">
            UAH
          </v-btn>
          <v-btn v-on:click="USD" flat value="USD">
            USD
          </v-btn>
          <v-btn v-on:click="EUR" flat value="EUR">
            EUR
          </v-btn>
        </v-btn-toggle>
        <v-card-text>КОЛЛИЧЕСТВО КОМНАТ</v-card-text>
          <div style="text-align: center;">
          <!-- <v-checkbox v-model="roomsvalueselected" label="ВСЕ" :value="(1|2|3)" @click="toggleAll"></v-checkbox> -->
          <v-checkbox v-model="roomsvalueselected" label="1 комната" :value="1"></v-checkbox>
          <v-checkbox v-model="roomsvalueselected" label="2 комнаты" :value="2"></v-checkbox>
          <v-checkbox v-model="roomsvalueselected" label="3 комнаты" :value="3"></v-checkbox>
          </div>
        <v-card-text>ЦЕНА </v-card-text>
        <v-layout>
        <v-flex xs6 >
        <v-text-field
          v-model="lowerprice"
          solo
          label="Минимальная цена"
          clearable
        ></v-text-field>
        </v-flex>
        <v-flex xs6>
        <v-text-field
          v-model="maxprice"
          solo
          label="Максимальная цена"
          clearable
        ></v-text-field>
        </v-flex>
        </v-layout>
        <!-- <v-flex class="px-3">
          <v-range-slider
            v-model="value3"
            :max="6000000"
            :min="20"
            :step="10"
          ></v-range-slider>
        </v-flex> -->
        <v-card-text>РЕЙТИНГ</v-card-text>
        <no-ssr>
        <star-rating v-model="ratingraw" @rating-selected="setRating" :show-rating="false" :glow="16" :rounded-corners="true" :star-size="50" :star-points="[23,2, 14,17, 0,19, 10,34, 7,50, 23,43, 38,50, 36,34, 46,19, 31,17]"></star-rating>
        </no-ssr>
        <v-btn @click="houseratingselected = []; ratingraw = 0; roomsvalueselected = [];maxprice = null; lowerprice = null; ">Обновить фильтры</v-btn>
      </v-card>
    </v-flex>  
    <v-flex xs12 sm9 md9 lg9 style="padding-top: 0px;" column>
      <slabcard
        v-for="slab in houses"
        :key="slab.id"
        :id="slab.id"
        :slabfrontimage="slab.images[0]"
        :price="slab.price * pricemodifier"
        :rooms="slab.total_rooms"
        :rating="slab.rating"
        :full_address="slab.full_address"
        :description="slab.description"
        :public_date="slab.public_date"
      />
    </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import StarRating from 'vue-star-rating'
import Logo from "~/components/Logo.vue";
import slabcard from "@/components/Slabcardall";
import vuestars from "@/components/VueStars";

export default {
  components: {
    Logo,
    slabcard,
    vuestars,
    StarRating
  },
  name: "app",

  mounted() {
    this.getHouses();
  },

  data() {
    return {   
      value3: [],
      checkbox: true,
      lowerprice: null,
      maxprice: null,
      text: "UAH",
      pricemodifier: 1,
      fetchedHouses: [],
      loading: false,
      roomsvalue: [1,2,3],
      roomsvalueselected: [],
      ratingraw: 0,
      houseratingselected: [],
      houserating: [1,2,3,4,5],
      
    };
  },

  methods: {
    setRating: function(ratingraw) {
      this.houseratingselected = [ratingraw]
    },
    USD () {
      this.pricemodifier = 27 
    },

    UAH () {
      this.pricemodifier = 1 
    },

    EUR () {
      this.pricemodifier = 31 
    },

    getHouses () {
      this.loading = true;

      this.$axios.get('http://demo4452328.mockable.io/property')
        .then(({ data }) => {
          this.loading = false;
          this.fetchedHouses = data.data;
          console.log(this.houses);
        })
        .catch(error => {
          this.loading = false;
        })
    },

    isShown (slab) {
      const { total_rooms, rating, price } = slab
      if (this.maxprice === null || this.maxprice.length === 0)
        return this.generatevalue.includes(total_rooms)
          && this.generaterating.includes(rating)
          && price * this.pricemodifier > this.lowerprice

      return this.generatevalue.includes(total_rooms)
          && this.generaterating.includes(rating)
          && price * this.pricemodifier > this.lowerprice
          && price * this.pricemodifier < this.maxprice
    },

    toggleAll () {
      this.roomsvalue = this.roomsvalue.length > 0 ? [] : [1,2,3]
    }
  }, 
  computed: {
    houses () {
      /* Filter fetched array here and return for render only needed items */
      if (this.roomsvalue.length === 0 && this.houseratingselected.length === 0 )
        return this.fetchedHouses

      return this.fetchedHouses.filter(x => this.isShown(x)) || []
    },

    generaterating() {
      const ratings = this.houserating.filter(value => this.houseratingselected.includes(value))
      return ratings.length > 0 ? ratings : [1,2,3,4,5]
    },
    generatevalue() {
      const rooms = this.roomsvalue.filter(value => this.roomsvalueselected.includes(value))
      return rooms.length > 0 ? rooms : [1,2,3]      
    }
  },

  watch: {}
};
</script>

<style>
.v-input--selection-controls {
  padding: 0px 0;
}
.v-input {
  margin-top: 0px;

}
.container.fluid {
  padding-top: 0px;
  max-width: 100%;
}
</style>
