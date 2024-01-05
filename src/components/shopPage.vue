<template>
    <div class="main">
        <div>
            <div class="selector__main"><select v-model="selectored" @change="() => {
                if (selectored == 0) {
                    page = 0
                    url = 'https://fakestoreapi.com/products'
                } else {
                    page = 0
                    url = `https://fakestoreapi.com/products/category/${categories[selectored]}`
                }
                getter()
            }">
                    <option value="0">Всё</option>
                    <option value="1">Электроника</option>
                    <option value="2">Ювелирные изделия</option>
                    <option value="3">Мужская одежда</option>
                    <option value="4">Женская одежда</option>
                </select></div>
            <div class="cards__list">
                <productCard v-for="index in 10" v-bind:key="index" :prop_name="name[index - 1]"
                    :prop_category="category[index - 1]" :prop_desc="desc[index - 1]" :prop_image="image[index - 1]"
                    :prop_price="price[index - 1]" :prop_rate="rate[index - 1]" :prop_rate_count="rate_count[index - 1]"
                    @aboutOpen="() => {
                        displayed = 1
                    }" @click="() => {
    name2 = name[index - 1]
    desc2 = desc[index - 1]
    price2 = price[index - 1]
    category2 = category[index - 1]
    image2 = image[index - 1]
    rate2 = rate[index - 1]
    rate_count2 = rate_count[index - 1]
}"></productCard>
            </div>

            <div class="main__pagination">
                <div class="prev" v-if="page == pages[0]" @click="() => {
                    page--
                    getter()
                }"><img src="../assets/dropdown_left.png" alt=""></div>
                <div class="pagination">
                    <p v-if="page - 2 >= 0" @click="() => {
                        page -= 2
                        getter()
                    }">{{ page - 1 }}</p>
                    <p v-if="page - 1 >= 0" @click="() => {
                        page--
                        getter()
                    }">{{ page }}</p>
                    <p style="background-color:rgb(142, 147, 196)">{{ page + 1 }}</p>
                    <p v-if="page + 1 < pages.length" @click="() => {
                        page++
                        getter()
                    }">{{ page + 2 }}</p>
                    <p v-if="page + 2 < pages.length" @click="() => {
                        page += 2
                        getter()
                    }">{{ page + 3 }}</p>
                </div>
                <div class="next" v-if="page != pages.length - 1" @click="() => {
                    page++
                    getter()
                }"><img src="../assets/dropdown_right.png" alt=""></div>
            </div>
        </div>
        <div class="absoluten">
            <productPage v-if="displayed == 1" class="productPage" :prop_name2="name2" :prop_desc2="desc2"
                :prop_price2="price2" :prop_category2="category2" :prop_image2="image2" :prop_rate2="rate2"
                :prop_rate_count2="rate_count2" :prop_display2="displayed" @adding="AddedToCart" @displaying="() => {
                    displayed = 0
                }">
            </productPage>
        </div>
        <div class="shop_cart">
            <button class="cart_btn" @click="() => {
                if (display_cart == 0) {
                    display_cart++
                } else {
                    display_cart--
                }
            }">Корзина</button>
            <div class="shop_cart__main" v-if="cart_amounts_parsed && display_cart == 1">
                <productInCart v-for="index_carts in cart_amounts_parsed.length" v-bind:key="index_carts"
                    :prop_amount3="cart_amounts_parsed[index_carts - 1]" :prop_image3="cart_images_parsed[index_carts - 1]"
                    :prop_name3="cart_names_parsed[index_carts - 1]" :prop_price3="cart_prices_parsed[index_carts - 1]"
                    :prop_index3="index_carts - 1" @deleting="deletingProd" @mounted1="summ_itog" @unmounted1="summ_itog_after_del"></productInCart>
                <div class="overall">
                    <p>Итоговая цена: {{ sum }} ₽</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import productCard from './productCard.vue'
import productPage from './productPage.vue'
import productInCart from './productInCart.vue'

export default {
    name: 'prodPage',
    components: {
        productCard,
        productPage,
        productInCart,
    },
    data() {
        return {
            display_cart: 0,
            i: 0,
            index: 0,
            index_carts: 0,
            page: 0,
            selectored: 0,
            categories: ['', 'electronics', 'jewelery', "men's clothing", "women's clothing"],
            name: [],
            desc: [],
            price: [],
            category: [],
            image: [],
            rate: [],
            rate_count: [],
            pages: [],
            amount_of_prod: 0,
            amount_of_prod2: 0,
            url: 'https://fakestoreapi.com/products',
            name2: 0,
            desc2: 0,
            price2: 0,
            category2: 0,
            image2: 0,
            rate2: 0,
            rate_count2: 0,
            pages2: 0,
            displayed: 0,
            cart_images: [],
            cart_names: [],
            cart_prices: [],
            cart_amounts: [],
            cart_images_parsed: [],
            cart_names_parsed: [],
            cart_prices_parsed: [],
            cart_amounts_parsed: [],
            deleted_price: 0,
            deleted_amount: 0,
            sum: 0,
        }
    },
    methods: {
        getter() {
            this.name = []
            this.desc = []
            this.price = []
            this.category = []
            this.image = []
            this.rate = []
            this.rate_count = []
            this.pages = []
            fetch(this.url)
                .then(response => response.json())
                .then(json => {
                    if ((json.length % 10) == 0) {
                        this.amount_of_prod = this.page * 10
                        this.amount_of_prod2 = (this.page + 1) * 10
                    } else {
                        if (json.length <= 10) {
                            this.amount_of_prod2 = json.length
                            this.amount_of_prod = 0
                        }
                        else {
                            this.amount_of_prod2 = this.amount_of_prod + 10
                            this.amount_of_prod = json.length % 10
                        }
                        console.log(this.amount_of_prod)
                        console.log(this.amount_of_prod2)
                    }
                    for (let i = this.amount_of_prod; i < this.amount_of_prod2; i++) {
                        this.name.push(json[i].title)
                        this.desc.push(json[i].description)
                        this.price.push(json[i].price)
                        this.category.push(json[i].category)
                        this.image.push(json[i].image)
                        this.rate.push(json[i].rating.rate)
                        this.rate_count.push(json[i].rating.count)
                    }
                    for (let j = 1; j <= Math.ceil(json.length / 10); j++) {
                        this.pages.push(j)
                    }
                    console.log(this.category)
                })

        },
        AddedToCart(image_2, name_2, price_2, amount_2) {
            this.cart_images.push(image_2)
            this.cart_names.push(name_2)
            this.cart_prices.push(price_2)
            this.cart_amounts.push(amount_2)
            const parsed = JSON.stringify(this.cart_images)
            const parsed2 = JSON.stringify(this.cart_names)
            const parsed3 = JSON.stringify(this.cart_prices)
            const parsed4 = JSON.stringify(this.cart_amounts)
            localStorage.setItem('cart_images', parsed)
            localStorage.setItem('cart_names', parsed2)
            localStorage.setItem('cart_prices', parsed3)
            localStorage.setItem('cart_amounts', parsed4)
            this.cart_images_parsed = JSON.parse(localStorage.getItem('cart_images'))
            this.cart_names_parsed = JSON.parse(localStorage.getItem('cart_names'))
            this.cart_prices_parsed = JSON.parse(localStorage.getItem('cart_prices'))
            this.cart_amounts_parsed = JSON.parse(localStorage.getItem('cart_amounts'))
            this.displayed = 0
        },
        deletingProd(index_2) {
            this.deleted_price = this.cart_prices_parsed[index_2]
            this.deleted_amount = this.cart_amounts_parsed[index_2]
            this.cart_images_parsed.splice(index_2, 1)
            this.cart_names_parsed.splice(index_2, 1)
            this.cart_prices_parsed.splice(index_2, 1)
            this.cart_amounts_parsed.splice(index_2, 1)
            this.cart_images.splice(index_2, 1)
            this.cart_names.splice(index_2, 1)
            this.cart_prices.splice(index_2, 1)
            this.cart_amounts.splice(index_2, 1)
            const parsed = JSON.stringify(this.cart_images_parsed)
            const parsed2 = JSON.stringify(this.cart_names_parsed)
            const parsed3 = JSON.stringify(this.cart_prices_parsed)
            const parsed4 = JSON.stringify(this.cart_amounts_parsed)
            localStorage.setItem('cart_images', parsed)
            localStorage.setItem('cart_names', parsed2)
            localStorage.setItem('cart_prices', parsed3)
            localStorage.setItem('cart_amounts', parsed4)
        },
        summ_itog(index) {
            this.sum += Math.ceil((this.cart_prices_parsed[index]*this.cart_amounts_parsed[index])*91)
        },
        summ_itog_after_del(){
            this.sum -= Math.ceil((this.deleted_price*this.deleted_amount)*91)
            this.deleted_price = 0
            this.deleted_amount = 0
        }
    },
    created() {
        this.getter()
    },
    mounted() {
        this.cart_images_parsed = JSON.parse(localStorage.getItem('cart_images'))
        this.cart_names_parsed = JSON.parse(localStorage.getItem('cart_names'))
        this.cart_prices_parsed = JSON.parse(localStorage.getItem('cart_prices'))
        this.cart_amounts_parsed = JSON.parse(localStorage.getItem('cart_amounts'))
    },
    computed: {
    }
}
</script>

<style lang="css">
.pagination {
    display: flex;
    flex-direction: row;
    gap: 10px;
}

.pagination p {
    font-size: 20px;
    width: 20px;
    text-align: center;
}

.absoluten {
    width: 100%;
    position: fixed;
    top: 0;
    z-index: 100;
    left: 0;
}

.main__pagination {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.main__pagination img {
    width: 20px;
}

.main {
    width: 80%;
    margin: 0 auto;
    min-height: 700px;
}

.cards__list {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    row-gap: 20px;
}

.selector__main {
    padding-top: 30px;
    margin-bottom: 30px;
    display: flex;
    flex-direction: row;
    justify-content: center;
}

.selector__main select {
    width: 300px;
    height: 50px;
    font-size: 20px;
}

.shop_cart {
    width: 25vw;
    position: absolute;
    top: 108px;
    z-index: 60;
    left: 70%;
}

.shop_cart__main {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 600px;
    overflow: scroll;
    overflow-x: hidden;
    background-color: white;
    border-radius: 0px 15px 15px 15px;
    border: 2px solid;
    padding-top: 20px;
    padding-bottom: 40px;
}

.shop_cart__main::-webkit-scrollbar {
    border-radius: 15px;
    width: 5px;
}

.shop_cart__main::-webkit-scrollbar-thumb {
    background-color: black;
    border-radius: 15px;
}

@media screen and (max-width: 992px) {
    .shop_cart {
        left: 40%;
        width: 50%
    }
}

.cart_btn {
    background-color: black;
    color: white;
    width: 150px;
    height: 21px;
    border-radius: 15px 15px 0px 0px;
}

.overall{
    position: absolute;
    width: 100%;
    display: flex;
    flex-direction: row;
    
    border: 2px solid;
    border-radius: 0px 0px 15px 15px;
    background-color: white;
    top: 631px;
    left: -1px;
}
</style>