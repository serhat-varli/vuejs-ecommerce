<template>
  <div>
    <Headers :changeBaskets="basketArry" />
    <HomeSlider />
    <HomeTree />
    <HomeProduct @adToBasket="changeBasket($event)" />
    <HomeLine/>
    <Footers />
  </div>
</template>


<script>
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
    Headers       : () => import("@/components/shared/header/headers"),
    Footers       : () => import("@/components/shared/footer/footer"),
    HomeSlider    : () => import("@/components/home/HomeSlider"),
    HomeTree      : () => import("@/components/home/HomeTree"),
    HomeProduct   : () => import("@/components/home/HomeProduct"),
    HomeLine      : () => import("@/components/home/HomeLine"),
  },
  data() {
    return {
      getProducts: [],
      basketArry: [],
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
  },

}
</script>
