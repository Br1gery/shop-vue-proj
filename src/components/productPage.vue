<template>
    <div class="darkness">
    </div>
    <div class="window1">
        <div class="main">
            <img :src="prop_image2" alt="">
            <div class="title__main">
                <h4>{{ prop_name2 }}</h4>
                <p>{{ prop_category2 }}</p>
            </div>
            <div class="desc__main">{{ prop_desc2 }}</div>
            <div class="icons__main">
                <div class="stars">
                    <div class="star__yellow" :style="{width : (prop_rate2 * 18) + 'px'}"></div>
                    <div class="star__white"></div>
                    <p>рейтинг: {{ prop_rate2 }}</p>
                </div>
                <p>Количество оценок: {{ prop_rate_count2 }}</p>
            </div>
            <div class="buy__zone">
                <p>{{ prop_price2 }}</p>
                <button class="buy">В корзину</button>
            </div>
        </div>
        <div class="absoluiten_2"><button @click="ClosePage">&#215;</button></div>
    </div>
</template>

<script>
export default {
    props: ['prop_name2', 'prop_desc2', 'prop_price2', 'prop_category2', 'prop_image2', 'prop_rate2', 'prop_rate_count2'],
    data() {
        return {

        }
    },
    methods: {
        ClosePage() {
            this.$emit('displaying')
            console.log(this.prop_price2)
        }
    },
    mounted() {
    },
}
</script>

<style lang="css" scoped>
.darkness {
    width: 100%;
    min-height: 1080px;
    opacity: 0.8;
    background-color: black;
    position: absolute;
    z-index: -1;
}

.window1 {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    max-height: 600px;
    margin: 0 auto;
    margin-top: 180px;
    background: #fff;
    border-radius: 10px;
    padding-top: 25px;
}

.window1 img {
    max-width: 200px;
}

p {
    z-index: 10;
    color: black;
}

.absoluiten_2 {
    position: absolute;
    margin-left: 450px;
}

.absoluiten_2 button {
    font-size: 50px;
    width: 50px;
    background-color: white;
    border: 0;
}

.star__yellow {
    position: absolute;
    background-image: url('../assets/Raiting_yellow.png');
    background-size: cover;
    height: 16px;
}

.star__white {
    width: 91px;
    height: 16px;
    background-image: url('../assets/Raiting_white.png');
}

.icons__main{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}

.desc__main{
    margin-bottom: 20px;
    text-align: center;
}

.stars p{
    margin: 0;
}

.buy__zone{
display: flex;
flex-direction: row;
justify-content: space-between;
align-items: center;
width: 60%;
}
.main{
    display: flex;
    flex-direction: column;
    align-items: center;
}

p,h4{
    text-align: center;
}

.buy__zone button{
    background-color: black;
    color: white;
    width: 150px;
    height: 30px;
    border-radius: 15px;
}
</style>