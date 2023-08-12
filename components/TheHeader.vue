<template>
    <header v-if="!hideHeaderOnPages.includes($route.name)">
        <div class="left">
            <NuxtLink to="/catalog">Каталог</NuxtLink>
            <NuxtLink to="/">Продавцу</NuxtLink>
        </div>
        <NuxtLink to="/">
            <img src="@/assets/img/headerlogo.svg" alt="">
        </NuxtLink>
        <div class="right">
            <div class="counter">
                <img src="@/assets/img/cart.svg" style="cursor: pointer;" @click="openCart()" alt="">
                <span v-if="cartLength > 0">{{ cartLength }}</span>
            </div>
            <div class="cart" :class="{ active: cartOpen }">

                <div class="name w-100 d-flex justify-content-between align-items-center">
                    <h3>корзина</h3>
                    <img src="@/assets/img/close.svg" style="cursor: pointer;" @click="cartOpen = false" alt="">
                </div>
                <div v-if="cart.length <= 0"></div>
                <div class="cartbody" v-else>
                    <div class="tru d-flex align-items-center justify-content-between" v-for="item in cart" :key="item.id">
                        <span>{{ item.products.name }}</span>
                        <div>
                            <small>
                                {{ item.products.price.toLocaleString() }} ₸
                            </small>
                            <img src="@/assets/img/mkn.svg" style="cursor: pointer;" @click="deleteFromCart(item.id)"
                                alt="">
                        </div>
                    </div>
                </div>

                <div class="d-flex justify-content-center buyProduct">
                    <button ref="buyBtn" @click="buyProduct()">перейти к оформлению</button>
                </div>
            </div>
            <div v-if="isAuth">
                <NuxtLink data-toggle="modal" data-target="#inModal">{{ userBalance }} ₸</NuxtLink>
                <NuxtLink to="/seller-account">Личный кабинет</NuxtLink>
            </div>
            <div v-else>
                <NuxtLink to="/login">Вход</NuxtLink>
                <NuxtLink to="/register">Регистрация</NuxtLink>
            </div>
        </div>

    </header>

    <TheOutModal></TheOutModal>
    <TheInModal></TheInModal>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios'
export default {
    mixins: [global],
    data() {
        return {
            hideHeaderOnPages: ['login', 'register'],
            testLogin: true,
            cartOpen: false,
            pathUrl: 'https://d-market.kz',
            userBalance: null,
            accountType: '',
            cartLength: localStorage.getItem('cartLength')
        }
    },
    methods: {
        openCart() {
            this.getCart()
            this.cartOpen = !this.cartOpen
        },

        getBuyer() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.userBalance = response.data.balance

                })
                .catch(error => console.log(error));
        },
        getSeller() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/seller/seller-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.userBalance = response.data.balance

                })
                .catch(error => console.log(error));
        },
        buyProduct() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/placed-basket`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            this.$refs.buyBtn.innerHTML = 'Оформляем'
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    if (response.status == 204) {
                        this.$refs.buyBtn.innerHTML = 'Недостаточно средств'
                    }
                    if (response.status == 201) {
                        this.getBuyer()
                        this.getCart()
                        this.$refs.buyBtn.innerHTML = 'Оплата прошла успешно!'
                    }

                    // window.location.href = '/buyer-account'
                })
                .catch(error => {
                    console.error(error)
                })
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            this.getBuyer()
            //this.getCart()
            this.accountType = 'buyer'
            setInterval(() => {
                this.cartLength = localStorage.getItem('cartLength')
            }, 1);
        }
        else if (accType == 'seller-account') {
            this.getSeller()
            this.accountType = 'seller'
        }
        else {
            console.log('not authorized')
        }

    }

}
</script>
<style lang="scss" scoped>
.buyProduct {
    margin-top: 37px;

    button {
        border-radius: 10px;
        border: 2px solid #000;
        padding: 10px 72px;
        background: transparent;
        font-size: 18px;
        font-style: normal;
        font-weight: 400;
        line-height: normal;
        text-transform: uppercase;
        color: #000;
        font-family: var(--cera);
        transition: all .3s ease;

        &:hover {
            color: #fff;
            background: #000;
        }
    }
}

.cartbody {
    max-height: 300px;
    overflow-y: auto;
    display: block !important;
}

.cartbody::-webkit-scrollbar {
    width: 0;
}

.active {
    // top: 10% !important;
    // left: 46.5% !important;
    transform: scaleY(1) !important;
}

.cart {
    position: fixed;
    padding: 36px;
    border-radius: 10px;
    background: #FFF;
    transform: scaleY(0);

    width: 540px;
    // top: -1000px;
    // left: 0;
    top: 10% !important;
    left: 46.5% !important;
    display: block !important;
    transition: all .3s ease;



    .tru {
        margin-top: 51px;


        span {
            font-size: 18px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            font-family: var(--cera);
            color: #000;
        }

        small {
            font-size: 20px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            color: #000;
            font-family: var(--cera);
        }

        div {
            display: flex;
            gap: 36px;
        }
    }

    .name {
        h3 {
            font-size: 32px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            text-transform: uppercase;
            font-family: var(--cera);
            color: #000;
        }

        img {
            cursor: pointer;
        }
    }
}

header {
    position: fixed;
    width: 100%;
    padding: 30px 150px;
    background: linear-gradient(285deg, #070707 0%, #262626 100%);
    z-index: 20;

    display: flex;
    justify-content: space-between;
    align-items: center;

    .left {
        display: flex;
        gap: 60px;

        a {
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            color: #fff;
            font-family: var(--cera);
            text-decoration: none;
        }
    }

    .right {
        display: flex;
        gap: 60px;
        position: relative;

        .counter {
            position: relative;

            span {
                position: absolute;
                background: #fff;
                right: -35%;
                top: -30%;
                color: #000;
                border-radius: 50%;
                padding: 4px 4px;

                font-size: 12px;
                font-style: normal;
                font-weight: 400;
                line-height: 110%;
                font-family: var(--cera);
            }
        }

        a {
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            color: #fff !important;
            font-family: var(--cera);
            text-decoration: none;
            cursor: pointer;
        }

        div {
            display: flex;
            gap: 60px;
        }
    }
}
</style>