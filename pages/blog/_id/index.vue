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
                                    <li>{{ title }}</li>
                                </ul>
                            </div>
                        </div>
                        <div class="col-md-9">
                            <div class="row">
                                <div class="col-12" v-for="(item, index) in getBlogs" :key="index">
                                    <div class="box__area">
                                        <div class="img___wrap">
                                            <h1>{{ item.title }}</h1>
                                            <img v-for="itemInner in item.image.slice(0, 1)" :src="`https://d-themes.com/vue/molla/server` + itemInner.url" :alt="item.name" :title="item.name" class="w-100" :key="itemInner.id" />
                                        </div>
                                        <div class="img___wrap">
                                            {{ item.content }}
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
            title : null
        }
    },
    methods: {
        getBlogDetail() {
            let slugId = this.$route.params.id
            axios.get("https://d-themes.com/vue/molla/server/blogs/classic?page=1&perPage=5")
                .then((response) => {
                    response.data.blogs.map((blog) => {
                        if(blog.slug == slugId) {
                            this.getBlogs.push(blog);
                            this.title = blog.title;
                        }
                    })
                    this.isLoading = false;
                })
                .catch((e) => {
                    console.log(e)
                })
        }
    },
    created() {
        this.getBlogDetail();
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
</style>