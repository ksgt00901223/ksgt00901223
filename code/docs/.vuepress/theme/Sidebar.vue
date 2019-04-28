<template>
    <div class="sidebar">
        <NavLinks/>
        <slot name="top"/>
        <div class="custom">
            <div class="avatar">
                <img :src="avatar" alt>
            </div>
            <p class="name">
                枫仁苑~长
                <i style="color:#409eff" class="iconfont icon-xingbienanxianxing"></i>
            </p>
            <!-- <p class="info">一级死肥宅，二把刀厨子，三流程序员</p> -->
            <p class="info" style="text-align:center">永远不要只满足于世界的表象，要敢于探寻未知的可能。</p>
            <p class="info" style="text-align:center">一流死肥宅</p>
            <p class="info" style="text-align:center">二把刀厨子</p>
            <p class="info" style="text-align:center">三档程序员</p>
            <!--             
            <p class="info"> <i class="iconfont icon-youxiang1"/> ：ksgt00901223@foxmail.com</p>
            <p class="info"> <i class="iconfont icon-dizhi"/> ：天津 滨海</p>
            <p class="info"> <i class="iconfont icon-segmentfault"/>：chow </p>
            <p class="info"> <i class="iconfont icon-yanzheng-jianshu"/>：平凡太郎</p>-->
        </div>
        <ul class="sidebar-links" v-if="items.length">
            <li v-for="(item, i) in items" :key="i">
                <SidebarGroup
                    v-if="item.type === 'group'"
                    :item="item"
                    :first="i === 0"
                    :open="i === openGroupIndex"
                    :collapsable="item.collapsable || item.collapsible"
                    @toggle="toggleGroup(i)"
                />
                <SidebarLink v-else :item="item"/>
            </li>
        </ul>
        <slot name="bottom"/>
    </div>
</template>

<script>
import SidebarGroup from "./SidebarGroup.vue";
import SidebarLink from "./SidebarLink.vue";
import NavLinks from "./NavLinks.vue";
import { isActive } from "./util";
import avatar from "../public/img/logo.gif";
export default {
    components: { SidebarGroup, SidebarLink, NavLinks },

    props: ["items"],

    data() {
        return {
            openGroupIndex: 0,
            avatar: avatar
        };
    },

    created() {
        this.refreshIndex();
    },

    watch: {
        $route() {
            this.refreshIndex();
        }
    },

    methods: {
        refreshIndex() {
            const index = resolveOpenGroupIndex(this.$route, this.items);
            if (index > -1) {
                this.openGroupIndex = index;
            }
        },

        toggleGroup(index) {
            this.openGroupIndex = index === this.openGroupIndex ? -1 : index;
        },

        isActive(page) {
            return isActive(this.$route, page.path);
        }
    }
};

function resolveOpenGroupIndex(route, items) {
    for (let i = 0; i < items.length; i++) {
        const item = items[i];
        if (
            item.type === "group" &&
            item.children.some(c => isActive(route, c.path))
        ) {
            return i;
        }
    }
    return -1;
}
</script>

<style lang="stylus">
@import './styles/config.styl';

.custom {
    padding: 2rem;

    .name {
        text-align: center;
        font-weight: 700;
        font-size: 1rem;
    }

    .info {
        font-size: 12px;
        // text-align center
        padding: 0 1rem;
        margin: 5px 0 0;
    }

    .avatar {
        width: 8rem;
        height: 8rem;
        margin: 0 auto;
    }
}

.sidebar {
    ul {
        padding: 0;
        margin: 0;
        list-style-type: none;
    }

    a {
        display: inline-block;
    }

    .nav-links {
        display: none;
        border-bottom: 1px solid $borderColor;
        padding: 0.5rem 0 0.75rem 0;

        a {
            font-weight: 600;
        }

        .nav-item, .repo-link {
            display: block;
            line-height: 1.25rem;
            font-size: 1.1em;
            padding: 0.5rem 0 0.5rem 1.5rem;
        }
    }

    .sidebar-links {
        padding: 1.5rem 0;
    }
}

@media (max-width: $MQMobile) {
    .sidebar {
        .nav-links {
            display: block;

            .dropdown-wrapper .nav-dropdown .dropdown-item a.router-link-active::after {
                top: calc(1rem - 2px);
            }
        }

        .sidebar-links {
            padding: 1rem 0;
        }
    }
}
</style>
