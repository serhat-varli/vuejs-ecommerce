<template>
  <div>
    <div v-if="isLoading">
      <div class="spinner"></div>
    </div>
    <template v-else>
      <Headers :changeBaskets="basketArry" />
      <HomeSlider />
      <HomeTree />
      <HomeProduct @adToBasket="changeBasket($event)" />
      <HomeLine />
      <Footers />
    </template>
  </div>
</template>


<script>
import { bus } from "../store/index"
export default {
  head() {
    return {
      title: "Molla",
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'My custom description'
        }
      ]
    }
  },
  components: {
    Headers: () => import("@/components/shared/header/headers"),
    Footers: () => import("@/components/shared/footer/footer"),
    HomeSlider: () => import("@/components/home/HomeSlider"),
    HomeTree: () => import("@/components/home/HomeTree"),
    HomeProduct: () => import("@/components/home/HomeProduct"),
    HomeLine: () => import("@/components/home/HomeLine"),
  },
  data() {
    return {
      getProducts: [],
      basketArry: [],
      isLoading: true
    }
  },
  methods: {
    changeBasket(addToBasket) {
      if (addToBasket != null) {
        if (addToBasket.length > 0) {
          this.basketArry.push(addToBasket)
        }
      }
    }
  },
  created() {
    bus.$on("adToBasket", (adToBasket) => {
      debugger
      //this.basketArry.push(adToBasket)
    })
    setTimeout(() => {
      this.isLoading = false;
    }, 2000)
  },

}
</script>

<style>
.spinner {
  width: 40px;
  height: 40px;
  margin: 100px auto;
  background-color: #333;
  border-radius: 100%;
  -webkit-animation: sk-scaleout 1.0s infinite ease-in-out;
  animation: sk-scaleout 1.0s infinite ease-in-out;
}

@-webkit-keyframes sk-scaleout {
  0% {
    -webkit-transform: scale(0)
  }

  100% {
    -webkit-transform: scale(1.0);
    opacity: 0;
  }
}

@keyframes sk-scaleout {
  0% {
    -webkit-transform: scale(0);
    transform: scale(0);
  }

  100% {
    -webkit-transform: scale(1.0);
    transform: scale(1.0);
    opacity: 0;
  }
}
</style>