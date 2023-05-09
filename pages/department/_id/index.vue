<template>
    <div>
        <Headers />
        <div class="container my-4">
            <div class="row">
                <div class="col-lg-12">
                    <div class="product__list">
                        <div class="breadcrumb w-100">
                            <ul>
                                <li>Home</li>
                                <li>Product</li>
                                <li>{{title}}</li>
                            </ul>
                        </div>
                        <div class="list" v-if="getLists.length > 0">
                            <div class="conteiner">
                                <div class="row">
                                    <div class="col-md-3"></div>
                                    <div class="col-md-9">
                                        <div class="row">
                                            <div class="items_area col-md-4" v-for="(item, index) in getLists" :key="index">
                                                <div class="img___wrap">
                                                    <img v-for="itemInner in item.pictures.slice(0, 2)"
                                                        :src="`https://d-themes.com/vue/molla/server` + itemInner.url"
                                                        :alt="item.name" :title="item.name" class="w-100"
                                                        :key="itemInner.id" />
                                                    <div name="basket" class="btn" @click="addBasket(item.id)" to="/"><i
                                                            class="icon-shopping-bag"></i> ADD TO CARD</div>
                                                </div>
                                                <div class="product__wrap">
                                                    <div class="product__category" v-if="item.category">
                                                        <nuxt-link v-for="category in item.category" :key="category.id"
                                                            :to="`/department/` + category.slug">{{ category.name
                                                            }}</nuxt-link>
                                                    </div>
                                                    <div class="product__info">
                                                        <nuxt-link :to="`/product/` + item.slug">{{ item.name
                                                        }}</nuxt-link>
                                                        <div class="price__area">
                                                            <span class="price" v-if="item.price">${{
                                                                item.price.toFixed(2).replace('.', ',') }}</span>
                                                            <span class="price__old" v-if="item.sale_price">${{
                                                                item.sale_price.toFixed(2).replace('.', ',') }}</span>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <Footers />
    </div>
</template>
  
  
<script>
import axios from "axios";
import 'vue-slick-carousel/dist/vue-slick-carousel.css';
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css';
export default {
    head() {
        return {
            title: "Molla - " + this.title,
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

    },
    data() {
        return {
            getLists: [],
            depName: null,
            title : null
        }
    },
    methods: {
        getDepartment() {
            let slugId = this.$route.params.id;
            axios.get("https://d-themes.com/vue/molla/server/shop")
                .then((response) => {
                    response.data.products.map((product) => {
                        product.category.map((cat) => {
                            if (cat.slug == slugId) {
                                this.getLists.push(product);
                                this.title = cat.name;
                            }
                        })
                    })
                })
                .catch((e) => {
                    console.log(e)
                })
        }
    },
    created() {
        this.getDepartment()
    },
}
</script>
  


<style scoped>
.product__list {width:100%;}
.product__list .breadcrumb {background:white;margin-left: 0;padding-left: 0;}
.product__list .breadcrumb ul {margin:0;padding: 0;display:flex;list-style: none;}
.product__list .breadcrumb ul li {margin-right:10px;font-size:16px;color: #777;}
.product__list .breadcrumb ul li::before {content:"\e909";font-family: 'icomoon';font-size: 10px;padding-right: 10px;}
.product__list .breadcrumb ul li:first-child::before {display: none;}
.product__list .breadcrumb ul li:last-child {color: #333;}
.product__list .items_area {position: relative;}
.product__list .items_area .img___wrap {position: relative;}
.product__list .items_area .img___wrap img {width: 100%;transition: all ease .3s;}
.product__list .items_area .img___wrap img:nth-last-child(2) {position: absolute;visibility: hidden;opacity:0;top:0;left:0;}
.product__list .items_area .img___wrap:hover img {visibility: hidden;opacity:0;}
.product__list .items_area .img___wrap:hover img:nth-last-child(2) {visibility: visible;opacity:1;}

</style>