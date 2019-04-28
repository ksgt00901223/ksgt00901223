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
            <h2>最近十篇</h2>
            <el-timeline>
                <el-timeline-item
                    v-for="(item, index) in listData"
                    :key="index"
                    :timestamp="lastTime(item.lastUpdated)"
                    placement="top"
                >
                    <el-card shadow="hover">
                        <div @click="goInfo(item.path,item.title)">
                            <h3 style="margin:1rem 0">{{item.title}}</h3>
                            <p style="margin:5px">
                                
                                <span
                                    :class="'fonta-color-'+setNum()"
                                    class="classify"
                                >{{item.frontmatter.classify}}</span>

                                
                                <!-- <span
                                    v-for="(t,i) in item.frontmatter.tags"
                                    :key="t"
                                    :class="'font-color-'+setNum()"
                                    class="tag"
                                >{{t}}</span>
                                <span style="float:right">提交于:{{lastTime(item.lastUpdated)}}</span> -->
                            </p>
                        </div>
                    </el-card>
                </el-timeline-item>
            </el-timeline>
        </div>

        <slot name="bottom"/>
    </div>
</template>

<script>
import { colors } from "../public/js/dataConfig.js";
export default {
    props: ["sidebarItems", "pathList"],

    data() {
        return {};
    },
    computed: {
        title() {
            return this.$page.frontmatter.title;
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
        },
        listData() {
            return this.pathList.slice(0, 10);
        }
    },

    methods: {
        lastTime(ns) {
            return new Date(ns).toLocaleString().replace(/:\d{1,2}$/, " ");
        },
        setNum() {
            return Math.floor(Math.random() * colors.length);
        },
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
        }
    }
};
</script>

<style lang="stylus"></style>
