<template>
    <div>
        <el-row :gutter="20">
                <el-col
                    v-for="(card,index) in cardData"
                    :key="card.title"
                    :xs="24"
                    :sm="24"
                    :md="12"
                    :lg="8"
                    class="card-item"
                >
                    <el-card shadow="hover" :class="'bg-color-'+index">
                        <router-link :to="{path:card.link,query:{classifyType:classifyType}} ">
                            <div :style="{'background':'url('+ card.img+')'}" class="cardBody">
                                <h3 style="color:#222">{{ card.title }}</h3>
                                <p style="color:#fff">{{ card.description }}</p>
                            </div>
                        </router-link>
                    </el-card>
                </el-col>
            </el-row>
    </div>
</template>

<script>
export default {
    props:['cardData'],
    data() {
        return {};
    }
};
</script>

<style lang="styl" scoped>
.bg {
    background: red;
}
.card-item {
    margin-bottom: 15px;
}
</style>