<template>
    <div>
        <Headers />
        <div class="container my-4">
            <div class="row">
                <div class="col-lg-9">
                    <template v-if="basketProduct.length > 0">
                        <div class="col-md-12">
                            <div class="row">
                                <div class="col-md-6">Product</div>
                                <div class="col-md-2">Price</div>
                                <div class="col-md-2">Quantity</div>
                                <div class="col-md-2">Total</div>
                            </div>
                        </div>
                        <div class="col-md-12" v-for="(item, index) in basketProduct" :key="index">
                            <div class="product__item">
                                <div class="row">
                                    <div class="col-md-6">
                                        <div class="product__title">
                                            <img v-for="itemInner in item.pictures.slice(0, 1)" :src="`https://d-themes.com/vue/molla/server` + itemInner.url" :alt="item.name" :title="item.name" width="60" height="80" :key="itemInner.id" />
                                            <nuxt-link :to="`product/` + item.slug">
                                                <h2>{{ item.name }}</h2>
                                            </nuxt-link>
                                        </div>
                                    </div>
                                    <div class="col-md-2">${{ item.price.toFixed(2).replace('.', ',') }}</div>
                                    <div class="col-md-2">{{ item.qty }}</div>
                                    <div class="col-md-2">${{ (item.qty * item.price).toFixed(2).replace('.', ',') }}</div>
                                </div>
                            </div>
                        </div>
                    </template>
                    <template v-else>
                        <div class="col-md-12">
                            <div class="empty__basket">
                                <i class="icon-shopping-cart"></i>
                                <p>No products added to the cart</p>
                            </div>
                        </div>
                    </template>
                </div>
                <div class="col-lg-3">
                    <div class="">
                        <Summarys />
                    </div>
                </div>
            </div>
        </div>
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
        Headers: () => import("@/components/shared/header/headers"),
        Footers: () => import("@/components/shared/footer/footer"),
        Summarys: () => import("@/components/shop/BasketSummary")
    },
    data() {
        return {
            basketCount: 0,
            basketProduct: [],
        }
    },
    methods: {
        getProduct() {
            let product = localStorage.getItem("cart")
            if (product != null) {
                let localStorageData = JSON.parse(product);
                localStorageData.map((product) => {
                    this.basketCount++;
                    this.basketProduct.push(product);
                    console.log(product)
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
            this.getProduct();
        }
    },
    created() {
        this.getProduct();
    },
}
</script>
  

<style scoped>
.product__item {width: 100%;border-top: 1px solid #ebebeb;margin-top: 15px;padding-top: 15px;}
.product__item .product__title {display: flex;align-items: center;}
.product__item .product__title img {margin-right: 15px;}
.product__item .product__title a {text-decoration: none;}
.product__item .product__title a h2 {font-size:16px;color:#333;}
.empty__basket {padding: 50px 0;text-align: center;}
.empty__basket i {font-size: 72px;margin-bottom: 36px;display: block;}
.empty__basket p  {color: #777;font-size: 20px;margin-bottom:30px;}
</style>