<template>
    <div class="page">
        <slot name="top"/>

        <Content
            :custom="false"
            ref="content"
            id="pageContent"
            :class="contentBG?contentBG:'defaultCover'"
            class="pageContent"
        />

        <div class="page-edit">
            <!-- <el-tag type="success" size="small">{{classify}}</el-tag> -->
            <span
                :class="'font-color-'+classifyBg+' bga-color-'+classifyBg"
                class="classify"
            >{{classify}}</span>
            <span
                v-for="(t,i) in tags"
                :key="t"
                :class="'font-color-'+(i+1)+' bga-color-'+(i+1)"
                class="tag"
            >{{t}}</span>
            <div class="last-updated" v-if="lastUpdated">
                <span
                    :id="$page.path"
                    class="leancloud-visitors"
                    data-flag-title="Your Article Title"
                >
                    <em class="post-meta-item-text">阅读量</em>
                    <i class="leancloud-visitors-count">0</i>；
                </span>
                <span class="prefix">{{ lastUpdatedText }}:</span>
                <span class="time">{{ lastUpdated }}</span>
            </div>
        </div>
        <div class="page-edit" v-if="paginationHidden">
            <el-button type="text" @click="preNext(0)" :disabled="index==0">
                <i class="el-icon-caret-left"></i>
                {{preBtn.txt}}
            </el-button>
            <el-button
                type="text"
                @click="preNext(1)"
                :disabled="index==listData.length-1"
                style="float:right"
            >
                {{nextBtn.txt}}
                <i class="el-icon-caret-right"></i>
            </el-button>
        </div>
        <div class="page-edit" v-if="!commentHidden">
            <comment-com></comment-com>
        </div>
        <!-- <div class="page-nav" v-if="prev || next"> -->
        <div class="page-nav" v-if="false">
            <p class="inner">
                <span v-if="prev" class="prev">
                    ←
                    <router-link
                        v-if="prev"
                        class="prev"
                        :to="prev.path"
                    >{{ prev.title || prev.path }}</router-link>
                </span>

                <span v-if="next" class="next">
                    <router-link v-if="next" :to="next.path">{{ next.title || next.path }}</router-link>→
                </span>
            </p>
        </div>

        <slot name="bottom"/>
    </div>
</template>

<script>
import { resolvePage, normalize, outboundRE, endingSlashRE } from "./util";
import { colors } from "../public/js/dataConfig.js";
import CommentCom from "../components/CommentCom";

export default {
    props: ["sidebarItems", "pathList"],
    comments: {
        CommentCom
    },
    data() {
        return {
            curList: [],
            classifyBg: this.setNum(),
            preBtn: {
                disabled: false,
                txt: "上一页"
            },
            nextBtn: {
                disabled: false,
                txt: "下一页"
            },
            index: 0
        };
    },
    created() {
        this.getIndex();
    },
    // watch: {
    //     $route(val, old) {
    //         if (val.path != old.path) {
    //             console.log(5665);
    //         } else {
    //         }
    //     }
    // },
    computed: {
        // 标签
        tags() {
            return this.$page.frontmatter.tags || [];
        },
        // 分类
        classify() {
            return this.$page.frontmatter.classify || "";
        },
        // 内容背景图
        contentBG() {
            return this.$page.frontmatter.contentBG;
        },
        commentHidden() {
            return this.$page.frontmatter.commentHidden;
        },
        lastUpdated() {
            if (this.$page.lastUpdated) {
                return new Date(this.$page.lastUpdated).toLocaleString(
                    this.$lang
                );
            }
        },
        listData() {
            const array = this.pathList;
            const tag = this.$route.query.tag || "";
            const classify = this.$route.query.classify || "";
            if (tag) {
                return array.filter(
                    v => v.frontmatter.tags && v.frontmatter.tags.includes(tag)
                );
            }
            if (classify) {
                return array.filter(
                    v =>
                        v.frontmatter.classify &&
                        v.frontmatter.classify === classify
                );
            }
            return this.pathList;
        },
        paginationHidden() {
            return !this.$page.frontmatter.paginationHidden;
        },

        lastUpdatedText() {
            if (typeof this.$themeLocaleConfig.lastUpdated === "string") {
                return this.$themeLocaleConfig.lastUpdated;
            }
            if (typeof this.$site.themeConfig.lastUpdated === "string") {
                return this.$site.themeConfig.lastUpdated;
            }
            return "Last Updated";
        },

        prev() {
            const prev = this.$page.frontmatter.prev;
            if (prev === false) {
                return;
            } else if (prev) {
                return resolvePage(this.$site.pages, prev, this.$route.path);
            } else {
                return resolvePrev(this.$page, this.sidebarItems);
            }
        },

        next() {
            const next = this.$page.frontmatter.next;
            if (next === false) {
                return;
            } else if (next) {
                return resolvePage(this.$site.pages, next, this.$route.path);
            } else {
                return resolveNext(this.$page, this.sidebarItems);
            }
        },

        editLink() {
            if (this.$page.frontmatter.editLink === false) {
                return;
            }
            const {
                repo,
                editLinks,
                docsDir = "",
                docsBranch = "master",
                docsRepo = repo
            } = this.$site.themeConfig;

            let path = normalize(this.$page.path);
            if (endingSlashRE.test(path)) {
                path += "README.md";
            } else {
                path += ".md";
            }
            if (docsRepo && editLinks) {
                return this.createEditLink(
                    repo,
                    docsRepo,
                    docsDir,
                    docsBranch,
                    path
                );
            }
        },

        editLinkText() {
            return (
                this.$themeLocaleConfig.editLinkText ||
                this.$site.themeConfig.editLinkText ||
                `Edit this page`
            );
        }
    },
    methods: {
        getIndex() {
            const title = this.$route.query.title;
            this.listData.forEach((v, i) => {
                if (v.title == title) {
                    this.index = i;
                }
            });
        },
        createEditLink(repo, docsRepo, docsDir, docsBranch, path) {
            const bitbucket = /bitbucket.org/;
            if (bitbucket.test(repo)) {
                const base = outboundRE.test(docsRepo) ? docsRepo : repo;
                return (
                    base.replace(endingSlashRE, "") +
                    `/src` +
                    `/${docsBranch}` +
                    (docsDir ? "/" + docsDir.replace(endingSlashRE, "") : "") +
                    path +
                    `?mode=edit&spa=0&at=${docsBranch}&fileviewer=file-view-default`
                );
            }

            const base = outboundRE.test(docsRepo)
                ? docsRepo
                : `https://github.com/${docsRepo}`;

            return (
                base.replace(endingSlashRE, "") +
                `/edit/${docsBranch}` +
                (docsDir ? "/" + docsDir.replace(endingSlashRE, "") : "") +
                path
            );
        },
        preNext(type) {
            if (type) {
                // console.log(type, "下一页");
                this.index += 1;
                const path = this.listData[this.index].path;
                const title = this.listData[this.index].title;
                this.goInfo(path, this.listData[this.index].title);
            } else {
                // console.log(type);
                this.index -= 1;
                const path = this.listData[this.index].path;
                const title = this.listData[this.index].title;
                this.goInfo(path, this.listData[this.index].title);
            }
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
        },
        setNum() {
            return Math.floor(Math.random() * colors.length);
        }
    }
};

function resolvePrev(page, items) {
    return find(page, items, -1);
}

function resolveNext(page, items) {
    return find(page, items, 1);
}

function find(page, items, offset) {
    const res = [];
    items.forEach(item => {
        if (item.type === "group") {
            res.push(...(item.children || []));
        } else {
            res.push(item);
        }
    });
    for (let i = 0; i < res.length; i++) {
        const cur = res[i];
        if (cur.type === "page" && cur.path === page.path) {
            return res[i + offset];
        }
    }
}
</script>

<style lang="stylus">
@import './styles/config.styl';
@require './styles/wrapper.styl';

.page {
    padding-bottom: 2rem;
}

.page-edit {
    @extend $wrapper;
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
    overflow: auto;

    .edit-link {
        display: inline-block;

        a {
            color: lighten($textColor, 25%);
            margin-right: 0.25rem;
        }
    }

    .last-updated {
        float: right;
        font-size: 0.9em;

        .prefix {
            font-weight: 500;
            color: lighten($textColor, 25%);
        }

        .time {
            font-weight: 400;
            color: #aaa;
        }
    }
}

.page-nav {
    @extend $wrapper;
    padding-top: 1rem;
    padding-bottom: 0;

    .inner {
        min-height: 2rem;
        margin-top: 0;
        border-top: 1px solid $borderColor;
        padding-top: 1rem;
        overflow: auto; // clear float
    }

    .next {
        float: right;
    }
}

@media (max-width: $MQMobile) {
    .page-edit {
        .edit-link {
            margin-bottom: 0.5rem;
        }

        .last-updated {
            font-size: 0.8em;
            float: none;
            text-align: left;
        }
    }
}
</style>
