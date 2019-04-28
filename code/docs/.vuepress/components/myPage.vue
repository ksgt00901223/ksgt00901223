<template>
    <div>
        <div  style="max-width:1200px;margin:0 auto;padding:2rem 0;background:#fff">
          <Content
            ref="content"
            id="pageContent"
            
            :class="contentBG?contentBG:'defaultCover'"
            class="pageContent"
        />  
        </div>
        
    </div>
</template>

<script>
    export default {
        computed: {
            // 内容背景图
        contentBG() {
            return this.$page.frontmatter.contentBG;
        },
        },
    }
</script>

<style lang="stylus" scoped>
div{
    text-align center
}
</style>