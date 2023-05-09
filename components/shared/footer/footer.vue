<template>
    <div>
        <div class="footer__wrap">
            <div class="container">
                <div class="row">
                    <div class="col-md-6">
                        <div class="footer__text">
                            <img src="https://d-themes.com/vue/molla/demo-8/images/logo-footer.png" width="82" height="22" alt="" />
                            <p>Praesent dapibus, neque id cursus ucibus, tortor neque egestas augue, eu vulputate magna eros
                                eu
                                erat. Aliquam erat volutpat. Nam dui mi, tincidunt quis, accumsan porttitor, facilisis
                                luctus,
                                metus.</p>
                                <p>
                                    <span class="d-block mb-3">Payment Method</span>
                                    <img src="https://d-themes.com/vue/molla/demo-8/images/payments.png" alt="">
                                </p>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="row">
                            <div class="col-md-4" v-for="(item, index) in footerNav" :key="index">
                                <div class="footer__nav">
                                    <h3>{{ item.header }}</h3>
                                    <ul>
                                        <li v-for="(itemInner, indexInner) in item.nav" :key="indexInner"><nuxt-link :to="itemInner.url">{{ itemInner.name }}</nuxt-link></li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-12">
                        <div class="footer__bottom">
                            <div class="row">
                                <div class="col-md-6">
                                    <p>Copyright © 2023 Molla Store. All Rights Reserved</p>
                                </div>
                                <div class="col-md-6">
                                  <div class="footer__social">
                                    <span>Social Media</span>
                                    <ul>
                                        <li><nuxt-link to="/" target="_blank"><i class="icon--facebook-logo"></i></nuxt-link></li>
                                        <li><nuxt-link to="/" target="_blank"><i class="icon-twitter"></i></nuxt-link></li>
                                        <li><nuxt-link to="/" target="_blank"><i class="icon--instagram-logo"></i></nuxt-link></li>
                                        <li><nuxt-link to="/" target="_blank"><i class="icon--youtube"></i></nuxt-link></li>
                                    </ul>
                                  </div>
                                </div>
                            </div>
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
            footerNav: [
                {
                    header: "INFORMATION",
                    nav: [
                        {
                            id: 1,
                            name: "About Molla",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 2,
                            name: "How to shop on Molla",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 3,
                            name: "FAQ",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 4,
                            name: "Contact us",
                            url: "#",
                            order: 1
                        }
                    ]
                },
                {
                    header: "CUSTOMER SERVICE",
                    nav: [
                        {
                            id: 1,
                            name: "Payment Methods",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 2,
                            name: "Money-back guarantee!",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 3,
                            name: "Returns",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 4,
                            name: "Shipping",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 5,
                            name: "Terms and conditions",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 6,
                            name: "Shipping",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 4,
                            name: "Privacy Policy",
                            url: "#",
                            order: 1
                        }
                    ]
                },
                {
                    header: "MY ACCOUNT",
                    nav: [
                        {
                            id: 1,
                            name: "About Molla",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 2,
                            name: "How to shop on Molla",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 3,
                            name: "FAQ",
                            url: "#",
                            order: 1
                        },
                        {
                            id: 4,
                            name: "Contact us",
                            url: "#",
                            order: 1
                        }
                    ]
                }
            ]
        }
    },
    methods: {
    },
    created() {
    },

}
</script>

<style scoped>
  .footer__wrap {width: 100%;font-weight: 300;font-size: 1.4rem;color: #777;background-color: #222;padding-top: 6rem;padding-bottom: 0.4rem }
  .footer__wrap .footer__nav {width:100%;}
  .footer__wrap .footer__nav h3 {font-size: 16px;color: white;margin-bottom: 10px;}
  .footer__wrap .footer__nav ul {margin: 0;padding: 0;list-style: none;width:100%}
  .footer__wrap .footer__nav ul li {margin:0;padding:0;}
  .footer__wrap .footer__nav ul li a {color: #777;text-decoration: none;font-size: 15px;margin: 0;padding: 0;}
  .footer__wrap .footer__text {width:100%;}
  .footer__wrap .footer__text img {margin-bottom:15px}
  .footer__wrap .footer__text p {font-size:16px}
  .footer__wrap .footer__bottom {border-top: 1px solid #444;width: 100%;margin-top: 20px;padding-top: 20px;}
  .footer__wrap .footer__bottom p {font-size:16px;}
  .footer__wrap .footer__social {display: flex;align-items: center;justify-content: end;}
  .footer__wrap .footer__social span {font-size:16px;margin-right:15px;}
  .footer__wrap .footer__social ul {list-style: none;display:flex;margin:0;padding: 0;}
  .footer__wrap .footer__social ul li  {display: inline-block;margin-right: 10px;}
  .footer__wrap .footer__social ul li:last-child {margin-right: 0px;}
  .footer__wrap .footer__social ul li a  {font-size:16px;margin:0;text-decoration: none;}
  .footer__wrap .footer__social ul li a i {font-size:16px;color:#777;}
</style>