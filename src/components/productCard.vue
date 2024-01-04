<template>
    <div class="card__main" v-if="prop_name">
        <img :src="prop_image" alt="" class="img__main">
        <div class="card__main__content">
            <h4>{{ prop_name }}</h4>
            <div class="icons">
                <p>Рейтинг: {{ prop_rate }}</p>
                <div class="rating">
                    <div class="rating__yellow" :style="{width : (prop_rate * 18) + 'px'}"></div>
                    <div class="rating__white"></div>
                </div>
                <button class="about_btn" @click="AboutOpen">Подробнее</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['prop_name', 'prop_desc', 'prop_price', 'prop_category', 'prop_image', 'prop_rate', 'prop_rate_count',],
    data() {
        return {
        }
    },
    mounted() {

    },
    methods:{
        AboutOpen(){
            this.$emit('aboutOpen')
        }
    }
}
</script>

<style lang="css">
.rating__yellow {
    position: absolute;
    background-image: url('../assets/Raiting_yellow.png');
    background-size: cover;
    height: 16px;
}

.rating__white {
    width: 91px;
    height: 16px;
    background-image: url('../assets/Raiting_white.png');
}

.img__main{
    max-width: 200px;
    height: 200px;
    object-fit: contain;
}

.icons{
    width: 400px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}

.card__main{
    height: 350px;
    width: 450px;
    padding-top: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    margin: 0 auto;
    background-color: white;
    border-radius: 20px;
}

.card__main h4{
    text-align: center;
}

.about_btn{
    background-color: black;
    color: white;
    width: 150px;
    height: 30px;
    border-radius: 15px;
}
</style>