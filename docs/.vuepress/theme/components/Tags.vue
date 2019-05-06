<!--  -->
<template>
    <div
        class='Tags'
        id='Tags'
    >
        <div class="tagsPart">
            <span style="font-size:12px">标签：</span>
            <span
                v-for='(value,key, index) of tags '
                :key='key'
                class="tagsItem  bg-colors"
                @click="emitData(key,value,index)"
                :class="active==index?'active':''"
            >
                <!-- <el-button
                    type="text"
                    :size="active===index?'medium':'mini'"
                    @click="emitData(key,value,index)"

                >{{key}}</el-button> -->
                {{key}}

            </span>
        </div>

    </div>
</template>

<script type="text/ecmascript-6">
export default {
    data() {
        return {
            active: 0
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

        tags() {
            return this.$tags._metaMap
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
            this.$emit('tagsItem', key, value)
        }
    }
}
</script>

<style scoped lang="stylus">
$lightColor = #eaecef;

#Tags {
    .tagsItem {
        padding: 4px 8px;
        margin-bottom: 5px;
        margin-right: 5px;
        cursor: pointer;
        font-size: 12px;
        color: #fff;
        transition: all 0.5s;
        display: inline-block;

        &:hover {
            transform: scale(1.2);
        }
    }

    .bg-colors:nth-child(8n) {
        background: #FC4349;
    }

    .bg-colors:nth-child(8n+1) {
        background: #CBD6DA;
    }

    .bg-colors:nth-child(8n+2) {
        background: #C458EC;
    }

    .bg-colors:nth-child(8n+3) {
        background: #FFE445;
    }

    .bg-colors:nth-child(8n+4) {
        background: #FCC143;
    }

    .bg-colors:nth-child(8n+5) {
        background: #FC4349;
    }

    .bg-colors:nth-child(8n+6) {
        background: #C458EC;
    }

    .bg-colors:nth-child(8n+7) {
        background: #FCC143;
    }
}
</style>
