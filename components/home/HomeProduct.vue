<template>
    <div>
        <div class="container mt-3">
            <div class="row">
                <div class="col-lg-12" v-if="getProducts[0]">
                    <div class="head__line">
                        <h2>FEATURED</h2>
                    </div>
                </div>
                <div class="col-lg-12" v-if="getProducts[0]">
                    <div class="home__product--slider">
                        <sliderProdcut class="slider" v-bind="sliderSetting">
                            <div class="slider__box--wrap" v-for="(item, index) in getProducts[0]" :key="index">
                                <div class="slider__box">
                                    <div class="img___wrap">
                                        <img v-for="itemInner in item.pictures.slice(0, 2)" :src="`https://d-themes.com/vue/molla/server` + itemInner.url" :alt="item.name" :title="item.name" class="w-100" :key="itemInner.id" />
                                        <div name="basket" class="btn" @click="addBasket(item.id)" to="/"><i class="icon-shopping-bag"></i> ADD TO CARD</div>
                                    </div>
                                    <div class="product__wrap">
                                        <div class="product__category" v-if="item.category">
                                            <nuxt-link v-for="category in item.category" :key="category.id" :to="`/department/` + category.slug">{{ category.name }}</nuxt-link>
                                        </div>
                                        <div class="product__info">
                                            <nuxt-link :to="`product/` + item.slug">{{ item.name }}</nuxt-link>
                                            <div class="price__area">
                                                <span class="price" v-if="item.price">${{ item.price.toFixed(2).replace('.', ',') }}</span>
                                                <span class="price__old" v-if="item.sale_price">${{ item.sale_price.toFixed(2).replace('.', ',') }}</span>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </sliderProdcut>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import 'vue-slick-carousel/dist/vue-slick-carousel.css';
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css';
import axios from "axios";
export default {
    components: {
        sliderProdcut: () => import('vue-slick-carousel'),
    },
    data() {
        return {
            getProducts: [],
            qty: 1,
            sliderSetting: {
                "dots": true,
                "dotsClass": "slick-dots custom-dot-class",
                "edgeFriction": 0.35,
                "infinite": true,
                "speed": 500,
                "slidesToShow": 4,
                "slidesToScroll": 1,
                responsive: [
                    {
                        breakpoint: 768,
                        settings: {
                            arrows: true,
                            slidesToShow: 2
                        }
                    }
                ]
            },
        }
    },

    methods: {
        addBasket(id) {
            debugger
            let cartIndex = undefined;
            const isLocalStorage = localStorage.getItem("cart");
            let localStorageData = JSON.parse(isLocalStorage);
            if (isLocalStorage != null) {
                cartIndex = localStorageData.findIndex(x => x.id == id);
            }
            if (isLocalStorage == null) {
                let cart = [];
                cart.push(this.getProducts[0].find(x => x.id == id))
                let cartIndex = cart.findIndex(x => x.id == id);
                cart[cartIndex]["qty"] = this.qty;
                localStorage.setItem("cart", JSON.stringify(cart));
            }
            else {
                let IsUndefined = cartIndex;
                if (IsUndefined == -1) {
                    let pushProduct = this.getProducts[0].find(x => x.id == id);
                    pushProduct["qty"] = this.qty;
                    localStorageData.push(pushProduct)
                }
                else {
                    localStorageData[cartIndex]["qty"] =  localStorageData[cartIndex].qty + 1
                }
                localStorage.setItem("cart", JSON.stringify(localStorageData));
            }
            this.$emit("adToBasket", localStorageData)
        },
        getProduct() {
            axios.get("https://d-themes.com/vue/molla/server/shop")
                .then((response) => {
                    this.getProducts.push(response.data.products);
                })
                .catch((e) => {
                    console.log(e)
                })
        },

    },
    created() {
        this.getProduct();
    },
 
}
</script>


<style scoped>
    .head__line {text-align: center;margin-top:15px;margin-bottom:30px;}
    .head__line h2 {border-bottom: 1px solid #333;display: inline-block;margin: 0 auto;text-align: center;font-weight: 600;font-size: 26px;letter-spacing: .01em;text-transform: uppercase;}
    .slider__box {position: relative;background-color: #fff;margin-bottom: 1rem;transition: box-shadow .35s ease;}
    .slider__box:hover {box-shadow: 0 5px 20px rgba(0,0,0,.05);}
    .slider__box .img___wrap {overflow: hidden;position: relative;}
    .slider__box .img___wrap img {transition: all ease .3s;opacity: 0;visibility: hidden;}
    .slider__box .img___wrap img:nth-child(1) {opacity: 1;visibility: visible;}
    .slider__box .img___wrap img:nth-child(2) {position: absolute;top: 0;bottom: 0;}
    .slider__box:hover .img___wrap img:nth-child(2) {opacity: 1;visibility: visible;}
    .slider__box:hover .img___wrap img:nth-child(1) {opacity: 0;visibility: hidden;}
    .slider__box .product__wrap {padding: 1.6rem 2rem;padding-bottom: 0.4rem;text-align: center;}
    .slider__box .product__category {display: flex;justify-content: center;}
    .slider__box .product__category a {color: #ccc;font-weight: 300;font-size: 16px;line-height: 1.2;letter-spacing: -.01em;margin-bottom: 0.3rem;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;margin-right: 10px;}
    .slider__box .product__category a:last-child {margin-right: 0px;}
    .slider__box .product__info a {font-weight: 400;font-size: 19px;line-height: 1.25;letter-spacing: -.01em;color: #333;margin-bottom: 5px;display: block;text-decoration: none;}
    .slider__box .product__info .price__area  {display: flex;align-items: center;flex-flow: wrap;font-weight: 400;font-size: 1.6rem;line-height: 1.25;margin-bottom: 1.3rem;color: #333;justify-content: center;}
    .slider__box .product__info .price__area span  {font-weight: 400;font-size: 19px;line-height: 1.25;letter-spacing: -.01em;color: #333;margin-bottom: 5px;display: block;text-decoration: none;}
    .slider__box .product__info .price__area span.price  {font-weight: 400;font-size: 19px;line-height: 1.25;letter-spacing: -.01em;color: #333;margin-bottom: 5px;display: block;text-decoration: none;margin-right: 15px;}
    .slider__box .product__info .price__area span.price__old  {font-weight: 400;font-size: 19px;line-height: 1.25;letter-spacing: -.01em;color: #333;margin-bottom: 5px;display: block;text-decoration: none;}
    .slider__box .btn {position: absolute;left: 0;right: 0;bottom: 0;z-index: 10;transition: all .35s ease;opacity: 0;visibility: hidden;transform: translateY(100%);background-color: #222;color: white;display: flex;align-items: center;justify-content: center;border-radius: 0;}
    .slider__box .btn i {margin-right:10px}
    .slider__box:hover .btn {transform: translateY(0);opacity: 1; visibility: visible;}
</style>