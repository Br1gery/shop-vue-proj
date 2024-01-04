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
                <option value="1">электроника</option>
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
                :prop_rate_count2="rate_count2" :prop_display2="displayed" @displaying="() => {
                    displayed = 0
                }">
            </productPage>
        </div>
    </div>
</template>

<script>
import productCard from './productCard.vue'
import productPage from './productPage.vue'

export default {
    name: 'prodPage',
    components: {
        productCard,
        productPage,
    },
    data() {
        return {
            i: 0,
            index: 0,
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

        }
    },
    created() {
        this.getter()
    }
}
</script>

<style lang="css">
.pagination {
    display: flex;
    flex-direction: row;
    gap: 10px;
}

.pagination p{
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

.main{
    width: 80%;
    margin: 0 auto;
    min-height: 700px;
}

.cards__list{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    row-gap: 20px;
}

.selector__main{
    padding-top: 30px;
    margin-bottom: 30px;
    display: flex;
    flex-direction: row;
    justify-content: center;
}

.selector__main select{
    width: 300px;
    height: 50px;
    font-size: 20px;
}
</style>