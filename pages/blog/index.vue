<template>
    <div>
        <div v-if="isLoading">
            <div class="spinner"></div>
        </div>
        <template v-else>
            <Headers />
            <div class="blog__list">
                <div class="container">
                    <div class="row">
                        <div class="col-md-12">
                            <div class="breadcrumb w-100">
                                <ul>
                                    <li>Home</li>
                                    <li>Blog</li>
                                    <li>Classic</li>
                                </ul>
                            </div>
                        </div>
                        <div class="col-md-9">
                            <div class="row">
                                <div class="col-12" v-for="(item, index) in getBlogs" :key="index">
                                    <div class="box__area">
                                        <div class="item___wrap">
                                            <nuxt-link class="title" :to="`/blog/` + item.slug"> <img v-for="itemInner in item.image.slice(0, 1)" :src="`https://d-themes.com/vue/molla/server` + itemInner.url" :alt="item.name" :title="item.name" class="w-100" :key="itemInner.id" /></nuxt-link>
                                            <ul class="item__info">
                                                <li>by {{ item.author }}</li>
                                                <li>{{ item.date }}</li>
                                                <li>{{ item.comments }} Comments</li>
                                            </ul>
                                            <nuxt-link class="title" :to="`/blog/` + item.slug">{{ item.title }}</nuxt-link>
                                            <ul class="category">
                                                <li>in</li>
                                                <li v-for="(innerItem, indexInner) in item.blog_categories" :key="indexInner"><nuxt-link :to="innerItem.slug">{{ innerItem.name }}</nuxt-link></li>
                                            </ul>
                                            <p>{{ item.content }}</p>
                                            <nuxt-link class="btn btn-warning" :to="`/blog/` + item.slug">Continue Reading</nuxt-link>
                                        </div>
                                        
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3"></div>
                    </div>
                </div>
            </div>
            <Footers />
        </template>
    </div>
</template>
  
<script>
import axios from "axios";
export default {
    head() {
        return {
            title: "Molla - Blog",
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
            isLoading: true,
            getBlogs: [],
        }
    },
    methods: {
        getBlog() {
            axios.get("https://d-themes.com/vue/molla/server/blogs/classic?page=1&perPage=5")
                .then((response) => {
                    response.data.blogs.map((blog) => {
                        this.getBlogs.push(blog)
                    })
                    this.isLoading = false;
                })
                .catch((e) => {
                    console.log(e)
                })
        }
    },
    created() {
        this.getBlog();
    },
}
</script>

<style scoped>
.blog__list {width:100%;}
.blog__list .breadcrumb {background:white;margin-left: 0;padding-left: 0;}
.blog__list .breadcrumb ul {margin:0;padding: 0;display:flex;list-style: none;}
.blog__list .breadcrumb ul li {margin-right:10px;font-size:16px;color: #777;}
.blog__list .breadcrumb ul li::before {content:"\e909";font-family: 'icomoon';font-size: 10px;padding-right: 10px;}
.blog__list .breadcrumb ul li:first-child::before {display: none;}
.blog__list .breadcrumb ul li:last-child {color: #333;}
.blog__list .box__area {width: 100%;}
.blog__list .box__area .item___wrap  {margin-bottom:30px;}
.blog__list .box__area .item___wrap .item__info {margin: 0;padding: 0;display: flex;align-items: center;margin-top: 20px;margin-bottom: 20px;}
.blog__list .box__area .item___wrap .item__info li {color: #777;list-style: none;margin-right: 15px;border-right: 1px solid #777;padding-right: 15px;line-height: 12px;}
.blog__list .box__area .item___wrap .item__info li:last-child {margin:0; padding: 0;}
.blog__list .box__area .item___wrap .title {color: #333;font-weight: 600;font-size: 26px;line-height: 1.25;letter-spacing: -.025em;margin-bottom: 0.6rem;display: block;width: 100%;text-decoration: none;}
.blog__list .box__area .item___wrap .category {display: flex;margin: 0;padding: 0;margin-bottom: 20px;}
.blog__list .box__area .item___wrap .category li  {margin-right: 15px;list-style: none;}
.blog__list .box__area .item___wrap .category li  {margin-right: 15px;list-style: none;color: #777;font-size: 18px;line-height: 1.25;letter-spacing: -.025em;display: block;font-weight: 400;text-decoration: none;}
.blog__list .box__area .item___wrap .category li:last-child  {margin-right: 15px;}
.blog__list .box__area .item___wrap .category li a  {color: #777;font-size: 18px;line-height: 1.25;letter-spacing: -.025em;display: block;width: 100%;margin: 0;font-weight: 400;text-decoration: none;}
</style>