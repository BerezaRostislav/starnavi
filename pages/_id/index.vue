<template>
<section class="single-slab">
  <h1> {{ id }} </h1>

</section>
</template>

<script>
import axios from 'axios';
  
export default {
  mounted () {
	  this.getslabs()
  },
  data () {
    return {
      id: this.$route.params.id,
      slabs: []
      ,
      loading: false,
      image: ""
    }
  }, 
  methods: {
    getslabs: function () {
      this.loading = true;
      axios({
        method: "GET",
        url: "http://demo4452328.mockable.io/property" + this.id + "/",
      }).then((response)  =>  {
        this.slabs = response.data.data
        this.image = response.data.card_front.image
        this.slabimage = response.data.card_front
        console.log(this.slabs.card_front.image)
       }, (error)  =>  {
        this.loading = false;
      })
    }
  },
}
</script>

<style scoped>

.slabfrontimage {
    background-position: center;
    background-size: cover;
    width: 265px;
    height: 460px;
}

h1 {
    color:black;
}

/* entire container, keeps perspective */
.flip-container {
	perspective: 1000px;
}
	/* flip the pane when hovered */
	.flip-container:hover .flipper, .flip-container.hover .flipper {
		transform: rotateY(180deg);
	}

.flip-container, .front, .back {
	width: 320px;
	height: 480px;
}

/* flip speed goes here */
.flipper {
	transition: 1.2s;
	transform-style: preserve-3d;

	position: relative;
}

/* hide back of pane during swap */
.front, .back {
	backface-visibility: hidden;

	position: absolute;
	top: 0;
	left: 0;

}

/* front pane, placed above back */
.front {
	z-index: 2;
	/* for firefox 31 */
	transform: rotateY(0deg);
}

/* back, initially hidden pane */
.back {
	transform: rotateY(180deg);
}
  body {
  margin: 0;
}
</style>

