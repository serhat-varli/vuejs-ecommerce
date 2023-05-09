<template>
    <div>
        <div class="container mt-3">
            <div class="row">
                <div class="col-sm-6 col-lg-4" v-for="(item, index) in threeBanner" :key="index">
                    <div class="banner__item">
                        <img :src="require(`~/` + item.image)" :alt="item.name" :title="item.name" class="w-100" />
                        <div class="inner__banner">
                            <nuxt-link to="/">
                                <h2>{{ item.name }}</h2>
                                <strong>{{ item.description }}</strong>
                                <small>{{ item.saleText }}</small>
                            </nuxt-link>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            threeBanner: [
                {
                    id: 1,
                    name: "Final reduction",
                    description: "Sandals & Flip Flops",
                    saleText: "up to 60% off",
                    image: "assets/image/banner-1.jpg"
                },
                {
                    id: 2,
                    name: "Limited time only.",
                    description: "Trainers & Sportwear",
                    saleText: "40 -70% off",
                    image: "assets/image/banner-2.jpg"
                },
                {
                    id: 3,
                    name: "This week we love...",
                    description: "Women's Accessories",
                    saleText: "from $6.99",
                    image: "assets/image/banner-3.jpg"
                }
            ]
        }
    },
    methods: {

    }
}
</script>

<style scoped>
    .banner__item {position: relative;overflow: hidden;}
    .banner__item .inner__banner {width:100%;position: absolute;top: 0;display: flex;align-items: center;flex-wrap: wrap;height: 100%;transition: all ease .4s;}
    .banner__item .inner__banner a {text-decoration: none;width:100%;height: 100%;padding: 40px;padding-top: 34%;position: relative;z-index: 10;}
    .banner__item .inner__banner h2 {font-weight: 300;text-transform: uppercase;margin-bottom: 10px;color: #777;font-size: 17px;}
    .banner__item .inner__banner  strong  {font-weight: 700;text-transform: uppercase;color: #333;display: block;font-size: 29px;}
    .banner__item .inner__banner  small  {color: #333;display: block;font-size: 25px;font-weight: 300;}
    .banner__item:hover .inner__banner::before {content: "";display: block;position: absolute;left: 0;right: 0;top: 0;bottom: 0;background-color: rgba(51,51,51,.25);z-index: 1;transition: all .4s ease;}
</style>