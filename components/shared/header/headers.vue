<template>
    <div>
        <div class="heade__wrap">
            <div class="container">
                <div class="row">
                    <div class="col-md-2">
                        <div class="logo">
                            <nuxt-link to="/">
                                <img src="https://d-themes.com/vue/molla/demo-8/images/home/logo.png" alt="" />
                            </nuxt-link>
                        </div>
                    </div>
                    <div class="col-md-7">
                        <nav>
                            <ul>
                                <li v-for="(item, index) in nav" :key="index"><nuxt-link :to="item.slug">{{ item.name }}</nuxt-link></li>
                            </ul>
                        </nav>
                    </div>
                    <div class="col-md-3">
                        <div class="site__header--icon">
                            <ul>
                                <li>
                                    <div class="search__icon" @click="searchClick"><i class="icon-search"></i></div>
                                    <div class="search__wrap" :class="searchOpen ? 'active' : ''">
                                        <form class="search__form">
                                            <span>Search</span>
                                            <input type="text" name="search" id="search" :value="search" placeholder="Search.." />
                                            <button @click="searchChange"><i class="icon-search"></i></button>
                                        </form>
                                        <i @click="searchClose" class="icon-close"></i>
                                    </div>
                                </li>
                                <li><nuxt-link to="/"><i class="icon-like"></i><small>{{ favoriteCount }}</small></nuxt-link></li>
                                <li class="small__wrap">
                                    <nuxt-link to="/cart"><i class="icon-shopping-bag1"></i><small>{{ basketCount }}</small></nuxt-link>
                                    <div class="small__basket" v-if="basketProduct.length > 0">
                                        <div class="small__basket--item--wrap">
                                            <div class="basket__item" v-for="(item, index) in basketProduct" :key="index">
                                                <nuxt-link :to="`/product/` + item.slug">
                                                    <div class="product__info">
                                                        <h2>{{ item.name }}</h2>
                                                        <h3>{{ item.qty }} x ${{ item.price.toFixed(2).replace('.', ',') }}
                                                        </h3>
                                                    </div>
                                                    <div class="img" v-for="(img, innerIndex) in item.sm_pictures.slice(0, 1)" :key="innerIndex">
                                                        <img :src="`https://d-themes.com/vue/molla/server` + img.url" :alt="item.name" />
                                                    </div>
                                                </nuxt-link>
                                                <i @click="removeItem(item.id)" class="icon-close"></i>
                                            </div>
                                        </div>
                                        <div class="btn__wrap">
                                            <span>Total ${{ totalPrice.toFixed(2).replace('.', ',') }}</span>
                                            <div class="btn__inner d-flex justify-content-between">
                                                <nuxt-link to="/cart" class="w-100 btn btn-outline-danger rounded-0 mr-2">Wiev Cart</nuxt-link>
                                                <nuxt-link to="/cart" class="w-100 btn btn-outline-info rounded-0">Checkout</nuxt-link>
                                            </div>
                                        </div>
                                    </div>
                                    <div v-else class="no__product small__basket">
                                        <p>No products in the cart.</p>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { bus } from "../../../store/index"
import 'assets/fonts/icomoon.css';
export default {
    name: 'Header',
    data() {
        return {
            basketCount: 0,
            basketProduct: [],
            favoriteCount: 0,
            totalPriceArry: [],
            totalPrice: 0,
            searchOpen: false,
            search: "",
            nav: [
                {
                    id: 1,
                    name: "Home",
                    slug: "/",
                    order: ""
                },
                {
                    id: 1,
                    name: "Shop",
                    slug: "/department/women",
                    order: ""
                },
                {
                    id: 1,
                    name: "Blog",
                    slug: "/",
                    order: ""
                },
                {
                    id: 1,
                    name: "Contact",
                    slug: "/",
                    order: ""
                }
            ]
        }
    },
    props: ["changeBaskets"],
    methods: {
        searchClick(e) {
            e.preventDefault();
            this.searchOpen = !this.searchOpen;
        },
        searchClose() {
            this.searchOpen = false;
        },
        searchChange(e) {
            e.preventDefault();
            alert(this.search)
        },
        getProduct() {
            let product = localStorage.getItem("cart");
            if (product != null) {
                let localStorageData = JSON.parse(product);
                localStorageData.map((product) => {
                    this.totalPriceArry.push({ id: product.id, price: product.price, qty: product.qty });
                    this.basketProduct.push(product);
                })
                this.totalPriceArry.map((price) => {
                    this.totalPrice += (price.qty * price.price);
                    this.basketCount += price.qty;
                })
            }
        },
        removeItem(id) {
            let removeData = localStorage.getItem("cart");
            let jsonData = JSON.parse(removeData)
            let productIndex = jsonData.findIndex(x => x.id === id);
            jsonData.splice(productIndex, 1)
            localStorage.setItem("cart", JSON.stringify(jsonData));
            this.basketCount = 0;
            this.basketProduct = [];
            this.totalPriceArry = [];
            this.totalPrice = 0;
            this.getProduct();
        }
    },
    created() {
        this.getProduct();

        bus.$on("adToBasket", (adToBasket) => {
            this.basketCount = 0;
            this.basketProduct = [];
            this.totalPriceArry = [];
            this.totalPrice = 0;
            this.getProduct();
        })
    },
    watch: {
        changeBaskets: function () {
            this.basketCount = 0;
            this.basketProduct = [];
            this.totalPriceArry = [];
            this.totalPrice = 0;
            this.getProduct();
        }
    }
}
</script>


<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
    .heade__wrap {font: normal 300 1.4rem/1.86 "Poppins", sans-serif;color: #666;background-color: #fff;position: fixed;z-index: 99;top: 0;width: 100%;}
    .heade__wrap .logo { height: 100%; display: flex; align-items: center;}
    .heade__wrap .logo img {width: 90px;}
    .heade__wrap nav { display: flex;align-content: center;justify-content:center;height:100%;}
    .heade__wrap nav ul { display: flex; flex-wrap: wrap;text-decoration: none;margin:0;padding: 0;}
    .heade__wrap nav ul li {display: flex;padding: 20px 0;margin-right: 30px;}
    .heade__wrap nav ul li:last-child {margin: 0;}
    .heade__wrap nav ul li a {display: flex;font-size: 16px;color: #222;position:relative;text-transform: uppercase;font-weight:500;font-family: "Poppins";}
    .heade__wrap nav ul li a:hover {text-decoration: none;}
    .heade__wrap nav ul li a:before {content: "";display: block;position: absolute;left: 0;width: 100%;height: 1px;transform-origin: right center;transform: scaleX(0);transition: transform .3s ease;    top: unset;bottom: -5px;background-color: #222;}
    .heade__wrap nav ul li a:hover:before {transform-origin: left center;transform: scale(1);}
    .heade__wrap .site__header--icon {display: flex;align-items: center;justify-content: end;height: 100%;}
    .heade__wrap .site__header--icon ul {display: flex;justify-content: end;align-items: center;margin: 0;}
    .heade__wrap .site__header--icon ul li {list-style: none;display: flex;align-items: center;justify-content: center;height: 100%;margin-right: 30px;}
    .heade__wrap .site__header--icon ul li:last-child {margin-right: 0px;}
    .heade__wrap .site__header--icon ul li a {position: relative;display: block;font-size: 20px;line-height: 1;font-weight: 400;color: #333;text-align: center;text-decoration: none;}
    .heade__wrap .site__header--icon ul li .search__icon {cursor:pointer;position: relative;display: block;font-size: 20px;line-height: 1;font-weight: 400;color: #333;text-align: center;text-decoration: none;}
    .heade__wrap .site__header--icon ul li a small {position: absolute;width: 17px;height: 17px;background-color: #222;color: #fff;font-weight: 400;font-size: 11px;display: flex;align-items: center;justify-content: center;border-radius: 100%;top: -6px;right: -10px;}
    .heade__wrap .site__header--icon .search__wrap {position: fixed;background: white;width: 100%;left: 0;right: 0;top: 0;z-index: 10;display: flex;align-items: center;justify-content: center;padding: 30px 0;transition: all ease .7s;transform: translate(0, -100%);}
    .heade__wrap .site__header--icon .search__wrap.active {transform: translate(0, 0);}
    .heade__wrap .site__header--icon .search__wrap .icon-close {position: absolute;right: 30px;cursor: pointer;font-size: 16px;font-weight: 500;color: black;}
    .heade__wrap .site__header--icon .search__wrap .search__form {display: flex;align-items: center;justify-content: center;}
    .heade__wrap .site__header--icon .search__wrap .search__form span {font-size: 20px;font-weight: 700;margin-right: 30px;}
    .heade__wrap .site__header--icon .search__wrap .search__form input {font-size: 20px;width: 450px;border: 0;margin-right: 0px;border-bottom: 1px solid #ccc;padding: 5px 30px 5px 15px;outline: 0;}
    .heade__wrap .site__header--icon .search__wrap .search__form input:focus {font-size: 20px;width: 450px;border: 0;margin-right: 0px;border-bottom: 1px solid #ccc;padding: 5px 30px 5px 15px;outline: 0;}
    .heade__wrap .site__header--icon .search__wrap .search__form button {display: flex;align-items: center;justify-content: center;border: 0;background: 0;font-size: 22px;margin-left: -30px;}
    .heade__wrap .site__header--icon .small__wrap .small__basket {background: white;visibility: hidden;opacity: 0;transition: ease all .2s;position: absolute;left: 0;transform: translate(0px, 20px);top:68px;z-index:99;width: 100%;}
    .heade__wrap .site__header--icon .small__wrap:hover .small__basket {opacity: 1;visibility: visible;transform: translate(0px, 0px);}
    .heade__wrap .site__header--icon .small__wrap .small__basket--item--wrap  {max-height: 325px;overflow-y: auto;display: block;width: 300px;padding: 30px;z-index: 100;font-size: 1.3rem;z-index: 1001;margin: 1px 0 0;border-radius: 0;border: none;padding-bottom: 0;}   
    .heade__wrap .site__header--icon .no__product p {font-size: 14px;text-align: center;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item {display: flex;align-items: center;border-bottom: 1px solid #ebebeb;padding-bottom: 10px;margin-bottom: 10px;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item a {display: flex;align-items: center;width: 100%;justify-content: space-between;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item .img img {width: 80px;padding-left: 15px;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item  i {cursor: pointer;font-size: 12px;margin-left: 8px;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item .product__info  {text-align: center;width: 100%;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item .product__info h2 {font-size:14px;margin:0 0 10px 0;width: 100%;}
    .heade__wrap .site__header--icon .small__wrap .small__basket .basket__item .product__info h3 {font-size:12px;margin:0;font-weight:600;color:black;width: 100%;}
    .heade__wrap .site__header--icon .small__wrap .btn__wrap {padding: 16px;}
    .heade__wrap .site__header--icon .small__wrap .btn__wrap span {font-size:16px;font-weight:600;}
    .heade__wrap .site__header--icon .small__wrap .btn__wrap .btn {font-size:16px}
</style>