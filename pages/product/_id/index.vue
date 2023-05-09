<template>
    <div>
        <Headers />
        <div class="container my-4">
            <div class="row">
                <div class="col-lg-12">
                    <div class="product">
                        <div class="breadcrumb w-100">
                            <ul>
                                <li>Home</li>
                                <li>Product</li>
                                <li>{{ name }}</li>
                            </ul>
                        </div>
                        <div class="row">
                            <div class="col-md-7">
                                <sliderProdcut v-if="img.length > 0" class="slider" v-bind="sliderSetting">
                                    <div v-for="item in img[0]" :key="item.id">
                                        <img :src="`https://d-themes.com/vue/molla/server` + item.url" :alt="name"
                                            :title="name" class="w-100" />
                                    </div>
                                </sliderProdcut>
                            </div>
                            <div class="col-md-5">
                                <div class="product__right">
                                    <h1>{{ name }}</h1>
                                    <div class="ratings__wrap">
                                        <div class="ratings">
                                            <span :style="`width:` + ratings + `%`" class="ratings-val"></span>
                                        </div>
                                        <span class="ratings-count" v-if="ratingsCount != null">( {{ ratingsCount }} Reviews )</span>
                                    </div>
                                    <div class="price">
                                        <span v-if="price != null">${{ price.toFixed(2).replace('.', ',') }}</span>
                                        <span v-if="oldprice != null">-</span>
                                        <span v-if="oldprice != null">${{ oldprice.toFixed(2).replace('.', ',') }}</span>
                                    </div>
                                    <div v-if="short_desc != null" class="description">
                                        <p> {{ short_desc }} </p>
                                    </div>
                                    <div class="color__wrap" v-if="variants != []"> 
                                        <span>Color : </span>
                                        <div class="color">
                                            <nuxt-link to="#" v-for="(item, index) in variants[0]" :key="index" :style="`background:` + item.color"></nuxt-link>
                                        </div>
                                    </div>
                                    <div class="qty__wrap">
                                        <span class="qty-text">Qty:</span>
                                        <div class="qty">
                                            <span @click="qtys('minus')" class="minus"><i class="icon-minus"></i></span>
                                            <span class="count">{{ qty }}</span>
                                            <span @click="qtys('plus')" class="plus"><i class="icon-plus"></i></span>
                                        </div>
                                    </div>
                                    <div class="btn__wrap">
                                        <div @click="addToBasket" class="btn btn-outline-danger rounded-0">ADD TO CART</div>
                                    </div>
                                    <div class="categry__wrap"> 
                                        <span>Category:</span>
                                        <nuxt-link v-for="category in category" :to="`/department/` + category.slug" :key="category.id">{{ category.name }},</nuxt-link>
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
        sliderProdcut: () => import('vue-slick-carousel'),
    },
    data() {
        return {
            getProducts: [],
            variants : [],
            img: [],
            name: null,
            price: null,
            oldprice: null,
            category: null,
            short_desc: null,
            ratings: 0,
            ratingsCount: 0,
            qty: 1,
            sliderSetting: {
                "dots": true,
                "dotsClass": "slick-dots custom-dot-class",
                "edgeFriction": 0.35,
                "infinite": true,
                "speed": 500,
                "slidesToShow": 1,
                "slidesToScroll": 1
            },
        }
    },
    methods: {
        getDetail() {
            let slugId = this.$route.params.id;
            axios.get("https://d-themes.com/vue/molla/server/shop")
                .then((response) => {
                    let product = response.data.products.find(x => x.slug == slugId)
                    this.getProducts.push(product);
                    this.img.push(product.pictures);
                    this.variants.push(product.variants)
                    this.name = product.name;
                    this.price = product.price;
                    this.oldprice = product.sale_price;
                    this.category = product.category;
                    this.short_desc = product.short_desc;
                    this.ratingsCount = product.ratings;
                    this.ratings = (product.ratings * 20);
                 
                    debugger
                })
                .catch((e) => {
                    console.log(e)
                })
        },
        qtys(qtyType) {
            if (this.qty != 0) {
                if (qtyType == "plus") {
                    this.qty += 1;
                }
                else {
                    this.qty -= 1;
                }
            }
            else {
                this.qty = 1;
            }
        },
        addToBasket(e) {
            debugger
            e.preventDefault();
            let getocalStorage = localStorage.getItem("cart");
            if (getocalStorage == null) {
                this.getProducts[0]["qty"] = this.qty;
                localStorage.setItem("cart", JSON.stringify(this.getProducts))
                this.$emit("adToBasket", this.getProducts);
            }
            else {
                let productPars = JSON.parse(getocalStorage)
                let productFind = productPars.find(x => x.id == this.getProducts[0].id)
                let IsUndefined = productFind;
                if (IsUndefined == undefined) {
                    this.getProducts[0]["qty"] = this.qty;
                    productPars.push(this.getProducts[0]);
                    localStorage.setItem("cart", JSON.stringify(productPars))
                    this.$emit("adToBasket", productPars);
                }
                else {
                    debugger
                    productFind["qty"] = productFind.qty + this.qty;
                    localStorage.setItem("cart", JSON.stringify(productPars))
                    this.$emit("adToBasket", productPars);
                }
            }

        }
    },
    created() {
        this.getDetail();
    },
}
</script>
  

<style scoped>
.product {width:100%;}
.product .breadcrumb {background:white;margin-left: 0;padding-left: 0;}
.product .breadcrumb ul {margin:0;padding: 0;display:flex;list-style: none;}
.product .breadcrumb ul li {margin-right:10px;font-size:16px;color: #777;}
.product .breadcrumb ul li::before {content:"\e909";font-family: 'icomoon';font-size: 10px;padding-right: 10px;}
.product .breadcrumb ul li:first-child::before {display: none;}
.product .breadcrumb ul li:last-child {color: #333;}
.product h1 {font-weight: 400;font-size: 28px;letter-spacing: -.090px;margin-bottom: 10px;margin-top: -6px;padding-right: 0;color: #333;line-height: 1.25;}
.product .price {margin-bottom:15px}
.product .price span {font-size: 22px;color: #eea287;}
.product .description {margin: 0;}
.product .description p {font-size: 15px;font-weight: 300;letter-spacing: 0;color: #777;margin: 0;}
.product .ratings__wrap  {position: relative;display: flex;margin-bottom: 5px;}
.product .ratings__wrap .ratings {position: relative;margin-right: 20px;}
.product .ratings__wrap .ratings::before {content:"\e90e  \e90e  \e90e  \e90e  \e90e";font-family: 'icomoon';color: #ccc;}
.product .ratings__wrap .ratings .ratings-val {position: absolute;top: 0;left: 0;white-space: nowrap;overflow: hidden;color: #fcb941;}
.product .ratings__wrap .ratings .ratings-val::before {content:"\e90d  \e90d  \e90d  \e90d  \e90d";font-family: 'icomoon';}
.product .ratings__wrap .ratings-count {color: #ccc;letter-spacing: -.01em;}
.product .color__wrap {display:flex;align-items: center;margin-top: 18px;}
.product .color__wrap  span {width: 67px;font-weight: 400;font-size: 16px;margin-bottom: 0;color: #666;}
.product .color__wrap  .color {display: flex;align-items: center;}
.product .color__wrap  .color a {width: 20px;height: 20px;margin-right: 15px;border-radius: 100%;align-items: center;transition: all ease .3s;}
.product .color__wrap  .color a:hover {box-shadow: 0 0 0 0.1rem #ccc;}
.product .qty__wrap {display:flex;align-items: center;margin-top: 30px;}
.product .qty__wrap .qty-text {width: 67px;font-weight: 400;font-size: 16px;margin-bottom: 0;color: #666;}
.product .qty__wrap .qty {max-width: 131px;flex: 0 0 131px;display: flex;align-items: center;justify-content: space-between;border: 1px solid #ccc;padding: 10px;}
.product .qty__wrap .qty .plus {cursor: pointer;}
.product .qty__wrap .qty .minus {cursor: pointer;}
.product .btn__wrap {margin-top: 15px;width: 100%;max-width: 200px;}
.product .btn__wrap div {width: 100%;cursor:pointer;}
.product .categry__wrap {width: 100%;border-top:1px solid #ebebeb;display: flex;margin-top: 3rem;padding: 20px 0;}
.product .categry__wrap a {color: #666; font-size:16px;margin-right:15px;}
.product .categry__wrap a:last-child {margin-right:0px;}
.product .categry__wrap  span {width: 90px;display:flex;color: #666; font-size:16px;}
.product .product__right {position:sticky; top:0;}
</style>