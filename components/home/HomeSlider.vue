<template>
    <div>
        <div class="home__slider">
            <slider class="slider" v-bind="sliderSetting">
                <div v-for="(items, index) in sliderAll" :key="index">
                    <img :src="require(`~/` + items.image)" :alt="items.name" :title="items.name" class="w-100" />
                </div>
            </slider>
        </div>
    </div>
</template>

<script>
import 'vue-slick-carousel/dist/vue-slick-carousel.css'
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'
export default {
    components: {
        slider: () => import('vue-slick-carousel'),
    },
    data() {
        return {
            sliderSetting: {
                "dots": true,
                "dotsClass": "slick-dots custom-dot-class",
                "edgeFriction": 0.35,
                "infinite": true,
                "speed": 500,
                "slidesToShow": 1,
                "slidesToScroll": 1
            },
            basePath: this.$config._app.assetsPath,
            sliderAll: [
                {
                    id: 1,
                    name: "slider 1",
                    image: "assets/image/img-1.png"
                },
                {
                    id: 2,
                    name: "slider 2",
                    image: "assets/image/img-2.png"
                }
            ]
        }
    },
    created() {
    }
}
</script>

<style scoped>
 .home__slider {width: 100%;overflow: hidden; }
</style>