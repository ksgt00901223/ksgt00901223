<template>
    <div class="page">
        <slot name="top"/>

        <Content
            v-if="contentShow"
            :custom="false"
            ref="content"
            id="pageContent"
            :class="contentBG?contentBG:'defaultCover'"
            class="pageContent"
        />
        <div :class="contentBG?contentBG:'defaultCover'" class="pageContent content">
            <div>
                <h2>{{ title }}</h2>
                <p>{{ description }}</p>
            </div>
            <div>
                <el-timeline>
                    <el-timeline-item
                        v-for="(item,index) in listData"
                        :key="index"
                        :color="colors[setNum()]"
                        :timestamp="lastTime(item.lastUpdated)"
                        placement="top"
                    >
                        <el-card shadow="hover">
                            <div @click="goInfo(item.path,item.title)" style="cursor: pointer;">
                                <h3>{{item.title}}</h3>
                                <p>
                                    <span
                                        style="display: inline-block;"
                                        :class="'fonta-color-'+setNum()"
                                        class="classify"
                                    >{{item.frontmatter.classify}}</span>
                                </p>
                            </div>
                        </el-card>
                    </el-timeline-item>
                </el-timeline>
            </div>
        </div>

        <slot name="bottom"/>
    </div>
</template>

<script>
import { colors, tags, classify } from "../public/js/dataConfig.js";

// import { resolvePage, normalize, outboundRE, endingSlashRE } from "./util";

export default {
    props: ["sidebarItems", "pathList"],
    data() {
        return {
            colors
        };
    },
    created() {},
    computed: {
        listData() {
            const array = this.pathList;
            const tag = this.$route.query.tag;
            const classify = this.$route.query.classify;
            if (tag) {
                return array.filter(
                    v => v.frontmatter.tags && v.frontmatter.tags.includes(tag)
                );
            }
            if (classify) {
                return array.filter(
                    v =>
                        v.frontmatter.tags &&
                        v.frontmatter.classify === classify
                );
            }
            return this.pathList;
        },
        title() {
            if (this.$route.query.classify) {
                return `分类：${this.$route.query.classify}文章列表`;
            }
            if (this.$route.query.tag) {
                return `标签：${this.$route.query.tag}文章列表`;
            }
            return `全部文章列表`;
        },
        description() {
            return this.$page.frontmatter.description;
        },
        contentShow() {
            return this.$page.frontmatter.contentShow;
        },
        // 内容背景图
        contentBG() {
            return this.$page.frontmatter.contentBG;
        }
    },
    methods: {
        goInfo(path, title) {
            const tag = this.$route.query.tag;
            const classify = this.$route.query.classify;
            this.$router.push({
                path: path,
                query: {
                    tag: tag,
                    title: title,
                    classify: classify
                }
            });
        },
        setNum() {
            return Math.floor(Math.random() * colors.length);
        },
        lastTime(ns) {
            return new Date(ns).toLocaleString().replace(/:\d{1,2}$/, " ");
        },
        bgColor(val) {
            return val % colors.length;
        }
    }
};
</script>

<style lang="stylus">
.list-item {
    margin-bottom: 1rem;
    cursor: pointer;

    .card {
        position: relative;
        height: 6rem;

        .prefix {
            position: absolute;
            left: 0;
            top: 0;
            bottom: 0;
            width: 1rem;
        }

        .suffix {
            height: 1.5rem;
            width: 1.5rem;
            bottom: 0;
            right: 0;
            position: absolute;
            border-top-left-radius: 1.5rem;
            -webkit-border-top-left-radius: 1.5rem;
        }

        .tu {
            height: 2rem;
            line-height: 2rem;
            text-align: center;
            width: 2rem;
            border-bottom-right-radius: 2rem;
            -webkit-border-bottom-right-radius: 2rem;
            top: 0;
            left: 0;
            color: #fff;
            position: absolute;
        }

        .cardContent {
            h3 {
                margin: 0;
                padding: 10px 5px;
                overflow: hidden;
                text-overflow: ellipsis; // 超出部分以省略号显示
                white-space: nowrap;
            }

            p {
                margin: 0;
                padding: 5px;
                font-size: 14px;
                // text-align: right;
            }
        }
    }
}
</style>
