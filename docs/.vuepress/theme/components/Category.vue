<!--  -->
<template>
    <div
        class='Category'
        id='Category'
    >
        <div>
            <span style="font-size:12px">
                分类：
            </span>
            <span
                class="categoryItem bg-colors"
                :class="active==-1?'active':''"
                style="border:0"
                @click="emitData('all',{},-1)"
            >
                全部
            </span>
            <span
                v-for='(value,key, index) of categories '
                :key='key'
                @click="emitData(key,value,index)"
                class="categoryItem bg-colors"
                :class="active==index?'active':''"
            >
                {{key}}
            </span>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
export default {
    data() {
        return {
            active: -1
        }
    },
    components: {},
    computed: {
        lastUpdated() {
            return this.$page.lastUpdated
        },

        lastUpdatedText() {
            if (typeof this.$themeLocaleConfig.lastUpdated === 'string') {
                return this.$themeLocaleConfig.lastUpdated
            }
            if (typeof this.$site.themeConfig.lastUpdated === 'string') {
                return this.$site.themeConfig.lastUpdated
            }
            return 'Last Updated'
        },
        categories() {
            return this.$categories._metaMap
        },
        colors() {
            return this.$themeConfig.colors
        }
    },

    methods: {
        randomNum() {
            return Math.floor(Math.random() * this.colors.length)
        },
        randomColor() {
            return this.colors[this.randomNum()]
        },
        emitData(key, value, active) {
            this.active = active
            this.$emit('categoryItem', key, value)
        }
    }
}
</script>

<style scoped lang="stylus">
$lightColor = #eaecef;
// '#FCC143', '#409EFF', '#67C23A',
#Category {
    .categoryItem {
        padding: 5px 10px;
        display inline-block
        margin-bottom: 5px;
        margin-right: 5px;
        cursor: pointer;
        font-size: 12px;
        color: #fff;
        transition: all 0.5s;

        &:hover {
            transform scale(1.2)
        }
    }

    .active {
        // font-size: 14px;
    }

    .bg-colors:nth-child(5n) {
        background: #FCC143;
    }

    .bg-colors:nth-child(5n+1) {
        background: #409EFF;
    }

    .bg-colors:nth-child(5n+2) {
        background: #67C23A;
    }

    .bg-colors:nth-child(5n+3) {
        background: #409EFF;
    }
    .bg-colors:nth-child(5n+4) {
        background: #67C23A;
    }
}
</style>
