<template>
    <div class="darkness">
    </div>
    <div class="window1">
        <div class="main__page__prod">
            <img :src="prop_image2" alt="" class="main__image">
            <div class="title__main">
                <h4>{{ prop_name2 }}</h4>
                <p>{{ prop_category2 }}</p>
            </div>
            <div class="desc__main">{{ prop_desc2 }}</div>
            <div class="icons__main">
                <div class="stars">
                    <div class="star__yellow" :style="{ width: (prop_rate2 * 18) + 'px' }"></div>
                    <div class="star__white"></div>
                    <p>рейтинг: {{ prop_rate2 }}</p>
                </div>
                <p>Количество оценок: {{ prop_rate_count2 }}</p>
            </div>
            <div class="buy__zone">
                <p>Цена: {{ Math.ceil(prop_price2 * 91) * amount }} ₽</p>
                <input type="number" v-model="amount" class="amount_prod" @change="()=>{
                    if(amount == 0){
                        amount = 1
                    }
                }">
                <button class="buy" @click="addToCart">В корзину</button>
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
            amount: 1
        }
    },
    methods: {
        ClosePage() {
            this.$emit('displaying')
        },
        addToCart(){
            this.$emit('adding',this.prop_image2,this.prop_name2,this.prop_price2,this.amount)
        }
    },
    mounted() {
    },
}
</script>

<style lang="css">
.darkness {
    width: 100%;
    min-height: 1080px;
    opacity: 0.8;
    background-color: black;
    position: absolute;
    z-index: -1;
}

.amount_prod{
    width: 50px;
}

.window1 {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    max-height: 650px;
    margin: 0 auto;
    margin-top: 180px;
    background: #fff;
    border-radius: 10px;
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

.icons__main {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}

.desc__main {
    margin-bottom: 20px;
    text-align: center;
}

.stars p {
    margin: 0;
}

.buy__zone {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 80%;
}

.main__page__prod {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 25px;
    justify-content: space-between;
    max-height: 650px;
    padding-bottom: 30px;
    padding-left: 30px;
    padding-right: 30px;
}

p,
h4 {
    text-align: center;
}

.buy__zone button {
    background-color: black;
    color: white;
    width: 150px;
    height: 30px;
    border-radius: 15px;
}

.main__image {
    max-width: 200px;
    height: 200px;
    object-fit: contain;
}

.buy:hover{
    background-color: rgb(63, 63, 63);
}
</style>