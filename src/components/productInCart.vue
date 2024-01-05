<template>
    <div class="main_cart">
        <img :src="prop_image3" alt="" class="main_image_cart">
        <div class="main_right_cart">
            <h5>{{ prop_name3 }}</h5>
            <div class="main_right_down_cart">
                <p>Кол-во: {{ prop_amount3 }}</p>
                <p>Цена: {{ Math.ceil(prop_price3 * 91 * prop_amount3) }} ₽</p>
            </div>
            <button @click="deleteProd" class="delete___btn">Удалить</button>
        </div>
    </div>
    <div class="empty" style="width: 100%;min-height: 40px;margin-top: 20px;"></div>
</template>
<script>
export default {
    props: ['prop_image3', 'prop_price3', 'prop_amount3', 'prop_name3', 'prop_index3'],
    mounted() {
        this.$emit('mounted1')
    },
    beforeUnmount() {
        this.$emit('unmounted1')
    },
    methods: {
        deleteProd() {
            this.$emit('deleting', this.prop_index3)
        }
    }
}
</script>
<style lang="css">
.main_image_cart {
    width: 100px;
    height: 100px;
    object-fit: contain;
}
.main_cart{
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    height: 75px;
}
.main_cart p{
    margin: 0;
}

.delete___btn{
    background-color: black;
    color: white;
    width: 150px;
    height: 30px;
    border-radius: 15px;
}
</style>