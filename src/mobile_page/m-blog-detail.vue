<template>
    <div class="blog">
        <div class="blog_title">{{blog.title}}</div>
        <div class="blog_msg">作者：WJL 标签：{{blog.tags}} 发布于：{{blog.ctime}} 浏览({{blog.views}})</div>
        <div class="blog_content markdown-body" v-html="blog.content"></div>
    </div>
</template>
<script>
import axios from '@/axios.js'
// import showdown from 'showdown'
import showdownHighlight from 'showdown-highlight'
import '@/assets/css/index/hljs.css'
import 'github-markdown-css'

export default {
    data() {
        return {
            blog: {}
        }
    },
    computed: {
        bid() { return this.$route.query.bid}
    },
    watch: {
        bid(oldVal, newVal) {
            this.init();
        }
    },
    methods: {
        init() {
            axios({
                method: "get",
                url: `/queryBlogById?bid=${this.bid}`
            }).then( res => {
                this.blog = res.data.data[0];
                const converter = new showdown.Converter({
                    extensions: [showdownHighlight]
                });
                this.blog.content = converter.makeHtml(this.blog.content);
            }).catch( error => {
                console.log("错误！")
            });
            
        }
    },
    created() {
        this.init();
    }
}
</script>

<style scoped lang="less">
/* @import url("../../../assets/css/index/makedown.css"); */
.blog {
    box-sizing: border-box;
    width: 7rem;
    background-color: #fff;
    border-radius: .06rem;
    padding: .15rem .2rem;
    margin: .2rem auto;
    box-shadow: 3px 4px 3px #888;
    .blog_title {
        font-size: .4rem;
        font-weight: 400;
        color: #13102b;
    }

    .blog_msg {
        padding-top: .1rem;
        padding-bottom: .2rem;
        border-bottom: 1px solid #424242;
        margin-bottom: .2rem;
        font-size: .24rem;
    }

    .blog_content {
        padding: .15rem .1rem;
        word-wrap: break-word;
        word-break: break-all;
        overflow: hidden;
    }
    .markdown-body {
        width: 95%;
    }
    a {
        text-decoration: none;
        color: #f66;
        border-bottom: 2px solid transparent;
        &:hover {
            color: #f33;
            border-bottom-color: #f33;
        }
    }
}

</style>