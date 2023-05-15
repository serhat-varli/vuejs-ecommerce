<template>
    <div>
        <div v-if="isLoading">
            <div class="spinner"></div>
        </div>
        <template v-else>
            <Headers />
            <div class="container my-4">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="product__list">
                            <div class="breadcrumb w-100">
                                <ul>
                                    <li>Home</li>
                                    <li>Product</li>
                                    <li>{{ title }}</li>
                                </ul>
                            </div>
                            <div class="list" v-if="getLists.length > 0">
                                <div class="conteiner">
                                    <div class="row">
                                        <div class="col-md-3">
                                            <div class="filter__item" v-if="getNewCategory[0].length > 0">
                                                <h2>Category</h2>
                                                <ul class="category">
                                                    <li v-for="(item, index) in getNewCategory[0]" :key="index"><nuxt-link :to="`/department/` + item.slug">{{ item.name }}</nuxt-link></li>
                                                </ul>
                                            </div>
                                            <div class="filter__item" v-if="getNewVariants[0].length > 0">
                                                <h2>Color</h2>
                                                <ul class="color">
                                                    <li v-for="(item, index) in getNewVariants[0]" :key="index"><nuxt-link to="#" :style="`background:` + item.name"></nuxt-link></li>
                                                </ul>
                                            </div>
                                            
                                            <div class="filter__item" v-if="getNewVariantsSize[0].length > 0">
                                                <h2>Size</h2>
                                                <ul class="size">
                                                    <li v-for="(item, index) in getNewVariantsSize[0]" :key="index"><nuxt-link to="#">{{ item.name }}</nuxt-link></li>
                                                </ul>
                                            </div>
                                        </div>
                                        <div class="col-md-9">
                                            <div class="row">
                                                <div class="items_area col-md-4" v-for="(item, index) in getLists"
                                                    :key="index">
                                                    <div class="img___wrap">
                                                        <img v-for="itemInner in item.pictures.slice(0, 2)" :src="`https://d-themes.com/vue/molla/server` + itemInner.url" :alt="item.name" :title="item.name" class="w-100" :key="itemInner.id" />
                                                        <div name="basket" class="btn" @click="addBasket(item.id)"><i class="icon-shopping-bag"></i> ADD TO CARD</div>
                                                    </div>
                                                    <div class="product__wrap">
                                                        <div class="product__category" v-if="item.category">
                                                            <nuxt-link v-for="category in item.category" :key="category.id" :to="`/department/` + category.slug">{{ category.name }}</nuxt-link>
                                                        </div>
                                                        <div class="product__info">
                                                            <nuxt-link :to="`/product/` + item.slug">{{ item.name }}</nuxt-link>
                                                            <div class="price__area">
                                                                <span class="price" v-if="item.price">${{ item.price.toFixed(2).replace('.', ',') }}</span>
                                                                <span class="price__old" v-if="item.sale_price">${{ item.sale_price.toFixed(2).replace('.', ',') }}</span>
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
        </template>
    </div>
</template>
  
  
<script>
import axios from "axios";
import { bus } from "../../../store/index"
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
            getCategorys: [],
            getNewCategory: [],
            getVariants: [],
            getNewVariants: [],
            getVariantsSize: [],
            getNewVariantsSize: [],
            depName: null,
            title: null,
            isLoading: true
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
                        if (product.variants.length > 0) {
                            product.variants[0].size.map((productsize, index) => {
                                this.getVariantsSize.push({ id: productsize.id, name: productsize.name })
                            })
                        }
                        if (product.variants.length > 0) {
                            product.variants.map((productColor, index) => {
                                this.getVariants.push({ id: productColor.id, name: productColor.color })
                            })
                        }
                        if (product.category.length > 0) {
                            product.category.map((productCategory, index) => {
                                this.getCategorys.push({ name: productCategory.name, slug: productCategory.slug })
                            })
                        }
                    })
                    let newMaping = [
                        {name: this.getCategorys, lasName : this.getNewCategory},
                        {name: this.getVariants, lasName : this.getNewVariants},
                        {name: this.getVariantsSize, lasName : this.getNewVariantsSize}
                    ];
                    newMaping.map((filterArray) => {
                        if (filterArray.name.length > 0) {
                            let newFilter = [];
                            filterArray.name.map((productFilter, index) => {
                                if (filterArray.lasName.length > 0) {
                                    let newItem = newFilter.filter(x => x.name == productFilter.name)
                                    if (newItem.length == 0) {
                                        newFilter.push({ id: productFilter.name,  name: productFilter.name, slug: productFilter.slug })
                                    }
                                }
                                else {
                                    newFilter.push({id: productFilter.name, name: productFilter.name, slug: productFilter.slug })
                                }
                                filterArray.lasName.push(newFilter)
                            })
                        }
                    })

                    this.isLoading = false;
                })
                .catch((e) => {
                    console.log(e)
                })
        },
        addBasket(id) {
            let getLocalStorage = localStorage.getItem("cart");
            if(getLocalStorage == null) {
                const product = this.getLists.filter(x => x.id == id)
                product[0]["qty"] = 1;
                localStorage.setItem("cart", JSON.stringify(product))
                bus.$emit("adToBasket", product);
            }
            else {
                const getLocalStorageParse = JSON.parse(getLocalStorage)
                const isProduct = getLocalStorageParse.filter(x => x.id == id);
                debugger
                if(isProduct.length == 0) {
                    const productItem = this.getLists.filter(x => x.id == id)
                    productItem[0]["qty"] = 1;
                     getLocalStorageParse.push(productItem[0])
                    localStorage.setItem("cart", JSON.stringify(getLocalStorageParse))
                }
                else {
                    isProduct[0].qty += 1;
                    localStorage.setItem("cart", JSON.stringify(getLocalStorageParse))
                }
                bus.$emit("adToBasket", getLocalStorageParse);
            }
           
        }
    },
    created() {
        this.getDepartment();

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
.product__list .items_area .img___wrap {overflow: hidden;position: relative;}
.product__list .items_area .img___wrap .btn  {position: absolute;left: 0;right: 0;bottom: 0;z-index: 10;transition: all .35s ease;opacity: 0;visibility: hidden;transform: translateY(100%);background-color: #222;color: white;display: flex;align-items: center;justify-content: center;border-radius: 0;}
.product__list .items_area .img___wrap .btn i {margin-right:10px;}
.product__list .items_area .img___wrap:hover .btn  {transform: translateY(0);opacity: 1; visibility: visible;}
.product__list .items_area .img___wrap img {width: 100%;transition: all ease .3s;}
.product__list .items_area .img___wrap img:nth-last-child(2) {position: absolute;visibility: hidden;opacity:0;top:0;left:0;}
.product__list .items_area .img___wrap:hover img {visibility: hidden;opacity:0;}
.product__list .items_area .img___wrap:hover img:nth-last-child(2) {visibility: visible;opacity:1;}
.product__list .items_area .product__wrap {padding: 1.6rem 2rem;padding-bottom: 0.4rem;text-align: center;}
.product__list .items_area .product__wrap .product__category {display: flex;justify-content: center;}
.product__list .items_area .product__wrap .product__category a {color: #ccc;font-weight: 300;font-size: 16px;line-height: 1.2;letter-spacing: -.01em;margin-bottom: 0.3rem;text-overflow: ellipsis;white-space: nowrap;overflow: hidden;margin-right: 10px;}
.product__list .items_area .product__wrap .product__category a:last-child {margin-right: 0;}
.product__list .items_area .product__wrap .product__info a {    font-weight: 400;font-size: 19px;line-height: 1.25;letter-spacing: -.01em;color: #333;margin-bottom: 5px;display: block;-webkit-text-decoration: none;text-decoration: none;}
.product__list .items_area .product__wrap .product__info .price__area {display: flex;align-items: center;flex-flow: wrap;font-weight: 400;font-size: 1.6rem;line-height: 1.25;margin-bottom: 1.3rem;color: #333;justify-content: center;}
.product__list .items_area .product__wrap .product__info .price__area span  {font-weight: 400;font-size: 19px;line-height: 1.25;letter-spacing: -.01em;color: #333;margin-bottom: 5px;display: block;-webkit-text-decoration: none;text-decoration: none;margin-right: 15px;}
.product__list .filter__item {margin-bottom: 30px;}
.product__list .filter__item h2 {color: #333;font-weight: 400;font-size: 20px;line-height: 47px;letter-spacing: -.01em;margin-bottom: 15px;border-bottom: 0.1rem solid #ebebeb;}
.product__list .filter__item ul {list-style: none;margin: 0;padding: 0;}
.product__list .filter__item .color {display: flex;align-items: center;list-style: none;margin: 0;padding: 0;flex-wrap: wrap;}
.product__list .filter__item .color a {width: 20px;height: 20px;margin-right: 15px;border-radius: 100%;align-items: center;transition: all ease .3s;display: block;margin-bottom:15px;}
.product__list .filter__item .color a:hover {box-shadow: 0 0 0 0.1rem #ccc;}
.product__list .filter__item .category a {font-size:16px;color: #333;text-decoration: none;}
</style>