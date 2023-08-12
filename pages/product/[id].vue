<template>
    <div class="product">
        <div class="empty" v-if="product.length <= 0"></div>
        <div class="product__body" v-else>
            <div class="product__side">
                <div class="name">
                    <h1>{{ product.name }}</h1>
                    <span>{{ product.price.toLocaleString() }} ₸</span>
                </div>

                <div class="product__description">
                    <h2>Описание:</h2>

                    <p v-html="addLineBreaks(product.description)"></p>

                </div>

            </div>
            <div class="seller__side">
                <div class="seller__info">
                    <small>Автор:</small>
                    <span>{{ seller.first_name }}</span>
                </div>
                <div class="seller__info">
                    <small>Категория:</small>
                    <span>{{ product.category.category_name }}</span>
                </div>
                <div class="seller__info">
                    <small>Регистратор домена:</small>
                    <span>{{ product.registrar }}</span>
                </div>
                <small ref="apiMessage" class="mb-2">{{ apiMessage }}</small>
                <button @click="addToCart(product.id)" ref="cartBtn">В КОРЗИНУ</button>


            </div>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            productId: this.$route.params.id,
            product: [],
            seller: [],
            pathUrl: 'https://d-market.kz',
            category: '',
            apiMessage: '',
        }
    },
    methods: {
        addToCart(id) {
            const path = `${this.pathUrl}/api/buyer/add-product-basket`
            axios
                .post(path, {
                    products: id,
                    amount: 1,
                })
                .then(response => {
                    if (response.status == 201) {
                        this.apiMessage = 'Товар успешно добавлен в корзину!'
                        this.$refs.apiMessage.classList.add('green')
                        this.$refs.cartBtn.disabled = true
                        this.$refs.cartBtn.classList.add('disabled')
                        this.getCart()
                    }
                    else {
                        this.apiMessage = 'Произошла ошибка, попробуйте еще раз'
                        this.$refs.apiMessage.classList.add('red')
                        this.$refs.cartBtn.disabled = false

                    }
                })
                .catch(error => {
                    console.error(error)
                })
        },
        getProduct() {
            const path = `${this.pathUrl}/api/products/detail-product/${this.productId}`
            axios
                .get(path)
                .then(response => {
                    this.product = response.data
                    this.seller = response.data.seller.user
                    this.category = response.data.category.category_name
                })
                .catch(error => {
                    console.error(error)
                })
        },
    },
    mounted() {
        this.getProduct()
    }
}
</script >
<script setup>
useSeoMeta({
    title: 'Товар | Domain Market',
    ogTitle: 'Товар | Domain Market',
    description: 'Товар | Domain Market',
    ogDescription: 'Товар | Domain Market',
})
</script>
<style lang="scss" scoped>
.green {
    color: green !important;
    font-family: var(--cera);
    font-size: 14px;
    display: block;
}

.red {
    color: red !important;
    font-family: var(--cera);
    font-size: 14px;
    display: block;
}

.disabled {
    background: grey !important;
    color: #fff !important;
}

.empty {
    height: 100vh;
}

.product {
    padding: 120px 150px;

    .product__body {
        margin-top: 50px;
        display: flex;
        justify-content: space-between;
        gap: 60px;

        .seller__side {
            width: 100%;
            max-width: 529px;

            .seller__info {
                display: flex;
                justify-content: space-between;
                align-items: center;
                margin-bottom: 24px;

                small {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--cera);
                    color: #fff;
                }

                span {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    color: #fff;
                    font-family: var(--cera);
                    padding: 7px 20px;
                    border-radius: 10px;
                    border: 2px solid #FFF;
                }
            }

            button {
                width: 100%;
                padding: 12px 0;
                border-radius: 10px;
                border: 2px solid #FFF;
                background: transparent;

                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 110%;
                color: #fff;
                font-family: var(--cera);
                transition: all .3s ease;

                &:hover {
                    background: #fff;
                    color: #000;
                }
            }
        }

        .product__side {
            width: 100%;
            max-width: 1041px;

            .name {
                display: flex;
                justify-content: space-between;
                align-items: center;

                h1 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--cera);
                    color: #fff;
                    margin: 0;
                }

                span {
                    font-size: 36px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--cera);
                    color: #fff;
                }
            }

            .product__description {
                margin: 43px 0 0;

                h2 {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    color: #fff;
                    font-family: var(--cera);
                }

                p {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--cera);
                    color: #fff;
                    max-width: 1041px;
                    margin-top: 10px;
                }
            }
        }
    }
}
</style>