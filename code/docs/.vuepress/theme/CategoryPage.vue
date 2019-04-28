<template>
    <div class="page">
        <slot name="top"/>
        <div :class="contentBG?contentBG:'defaultCover'" class="pageContent content">
            <div>
                <h2>
                    标签
                    <i :class="'font-color-'+setNum()" class="iconfont icon-tags"></i>
                </h2>
                <p>共 {{tagsData.length}}个标签</p>
                <div style="word-wrap:break-word;">
                    <router-link
                        :to="{path:'/article/',query:{tag:item} }"
                        v-for="(item, index) in tagsData"
                        :key="index"
                    >
                        <!-- <span :class="'bg-color-'+setNum()" class="tag" >{{item}}</span> -->
                        <span
                            :style="{'font-size':setNum()+14+'px'}"
                            :class="'font-color-'+setNum()"
                            style="padding:2px 3px;margin:2px 5px;display:inline-block"
                        >{{item}}</span>
                    </router-link>
                </div>
            </div>
            <div>
                <h2>
                    分类
                    <i
                        :class="'font-color-'+setNum()"
                        class="iconfont icon-leimupinleifenleileibie"
                    ></i>
                </h2>
                <p>共{{cardData.length}}个分类</p>
                <el-row :gutter="20">
                    <el-col
                        v-for="(card,index) in cardData"
                        :key="card.title"
                        :xs="24"
                        :sm="24"
                        :md="12"
                        :lg="8"
                        :xl="6"
                        class="card-item"
                    >
                        <el-card
                            shadow="hover"
                            :class="'bg-color-'+bgColor(index)"
                            style="height:10rem"
                        >
                            <router-link :to="{path:'/article/',query:{classify:card.title} } ">
                                <div :style="{'background':'url('+ card.img+')'}" class="cardBody">
                                    <h3 style="color:#222">
                                        <i :class="card.icon" class="iconfont"></i>
                                        {{ card.title }}
                                    </h3>
                                    <p style="color:#fff">{{ card.description }}</p>
                                </div>
                            </router-link>
                        </el-card>
                    </el-col>
                </el-row>
            </div>
        </div>

        <Content
            v-if="contentShow"
            :custom="false"
            ref="content"
            id="pageContent"
            :class="contentBG?contentBG:'defaultCover'"
            class="pageContent"
        />

        <slot name="bottom"/>
    </div>
</template>

<script>
import { colors, tags, classify } from "../public/js/dataConfig.js";

export default {
    props: ["sidebarItems", "pathList"],
    data() {
        return {
            tagsData: tags
        };
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
        classifyType() {
            return this.$page.frontmatter.type;
        },
        cardData() {
            return this.$page.frontmatter.listData;
        }
    },

    methods: {
        setNum() {
            return Math.floor(Math.random() * colors.length);
        },
        bgColor(val) {
            return val % colors.length;
        }
    }
};
</script>

<style lang="stylus">
.card-item {
    margin-bottom: 1rem;
    cursor: pointer;
}

.cardBody {
    background-size: cover;
}
</style>
