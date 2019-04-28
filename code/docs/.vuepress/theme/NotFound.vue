<template>
    <div class="theme-container">
        <div class="content">
            <!-- <h1>404</h1> -->
            <div class="box-404">
                <img v-if="num!=0" src="https://s2.ax1x.com/2019/04/13/ALbaKe.png" alt>
                <img v-else src="https://s2.ax1x.com/2019/04/13/ALLHuF.png" alt>
                <p>{{ getMsg() }}</p>
            </div>
            <p>
                <router-link to="/">
                    <i class="el-icon-back"></i> 回到首页
                </router-link>
            </p>
        </div>
    </div>
</template>

<script>
const msgs = [
    ``,
    `麋鹿了，快回家！`,
    `内容变成蝴蝶飞走了！`,
    `我不是你老婆，快走开！`,
    `白纸一张:)`,
    `傻瓜才看不见:)`,
    `跟你钱包一样白:)`
];

export default {
    data() {
        return {
            num: 0
        };
    },
    methods: {
        getMsg() {
            this.num = Math.floor(Math.random() * msgs.length);
            // console.log(this.num);
            return msgs[this.num];
        }
    }
};
</script>
<style lang="stylus" scoped>
.box-404 {
    position: relative;
    // width: 640px;
    // height: 560px;
    max-width: 100%;
    display: inline-block;

    img {
        max-width: 100%;
        width: 640px;
    }

    p {
        position: absolute;
        top: 70%;
        left: 60%;
        width 50%;
        transform translate(-50%,-50%)
        font-size: 2rem
        margin 0
    }
}
</style>

