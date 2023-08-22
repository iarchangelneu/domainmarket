<template>
    <header v-if="!hideHeaderOnPages.includes($route.name)" :style="headerStyle">
        <div class="pc">
            <div class="left">
                <NuxtLink to="/catalog">Каталог</NuxtLink>
                <NuxtLink to="/for-seller">Продавцу</NuxtLink>
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
                        <div class="tru d-flex align-items-center justify-content-between" v-for="item in cart"
                            :key="item.id">
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
                    <NuxtLink data-toggle="modal" data-target="#inModal">{{ Math.floor(userBalance) }} ₸</NuxtLink>
                    <NuxtLink :to="accountUrl">Личный кабинет</NuxtLink>
                </div>
                <div v-else>
                    <NuxtLink to="/login">Вход</NuxtLink>
                    <NuxtLink to="/register">Регистрация</NuxtLink>
                </div>
            </div>
        </div>

        <div class="mob">
            <NuxtLink to="/">
                <img src="@/assets/img/headermob.svg" alt="">
            </NuxtLink>

            <div class="rightblock">
                <div class="counter">
                    <img src="@/assets/img/cartmob.svg" style="cursor: pointer;" @click="openCart()" alt="">
                    <span v-if="cartLength > 0">{{ cartLength }}</span>
                </div>

                <div>
                    <input id="menu__toggle" type="checkbox" />
                    <label class="menu__btn" for="menu__toggle">
                        <span></span>
                    </label>


                    <ul class="menu__box">
                        <div class="mobmenu">
                            <div class="links text-center">

                                <NuxtLink to="/catalog">Каталог</NuxtLink>
                                <NuxtLink to="/for-seller">Продавцу</NuxtLink>
                                <div v-if="!isAuth">
                                    <NuxtLink to="/login">Войти</NuxtLink>
                                    <NuxtLink to="/register">регистрация</NuxtLink>
                                </div>
                                <div v-else>
                                    <NuxtLink style="cursor: pointer;" alt="" data-toggle="modal"
                                        :data-target="accountType === 'seller' ? '#outModal' : '#inModal'">{{
                                            Math.floor(userBalance) }} ₸</NuxtLink>
                                    <NuxtLink :to="accountUrl">Личный кабинет</NuxtLink>
                                </div>

                                <div class="text-center">
                                    <img src="@/assets/img/mobcal.svg" alt="">
                                </div>
                            </div>
                        </div>
                    </ul>
                </div>
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
            cartLength: localStorage.getItem('cartLength'),
            scrollPosition: 0,
        }
    },
    computed: {
        headerStyle() {
            if (this.scrollPosition > 100) { // Измените этот порог по своему усмотрению
                return {
                    background: 'linear-gradient(285deg, #070707 0%, #262626 100%)',
                };
            } else {
                return {
                    background: 'transparent',
                };
            }
        },
    },
    methods: {
        handleScroll() {
            this.scrollPosition = window.scrollY;
        },
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
    beforeDestroy() {
        window.removeEventListener('scroll', this.handleScroll);
    },
    mounted() {

        window.addEventListener('scroll', this.handleScroll);
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

        @media (max-width: 1024px) {
            font-size: 16px;
            padding: 10px 0;
            width: 100%;
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

    @media (max-width: 1024px) {
        width: 100%;
        left: 0 !important;
        padding: 20px;
    }

    .tru {
        margin-top: 51px;

        @media (max-width: 1024px) {
            flex-direction: column;
            align-items: flex-start !important;
            justify-content: flex-start !important;
            margin-top: 20px;
        }


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
            gap: 0;
            margin-top: 10px;

            @media (max-width: 1024px) {
                justify-content: space-between;
                width: 100%;
            }
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
    background: transparent;
    z-index: 20;


    @media (max-width: 1500px) {
        padding: 30px 50px;
    }

    @media (max-width: 1024px) {
        padding: 30px 20px;
    }

    .mob {
        display: none;
        align-items: center;
        justify-content: space-between;
        position: relative;

        @media (max-width: 1024px) {
            display: flex;
        }

        .rightblock {
            display: flex;
            gap: 30px;
            align-items: center;

            .counter {
                margin-top: 5px;
                position: relative;

                span {
                    position: absolute;
                    background: #fff;
                    right: 0%;
                    top: 0%;
                    color: #000;
                    border-radius: 50%;
                    padding: 2px 5px;

                    font-size: 12px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 110%;
                    font-family: var(--cera);
                }
            }
        }
    }

    .pc {
        display: flex;
        justify-content: space-between;
        align-items: center;

        @media (max-width: 1024px) {
            display: none;
        }

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
                    padding: 2px 5px;

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
}

.mobmenu .links img {
    margin-top: 150px;
}

.mobmenu .links a {
    display: block;
    margin-bottom: 30px;
    font-size: 36px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--cera);
    color: #fff;
    text-decoration: none;
}

.links {
    margin-top: 50px;
}

.menu__box a:hover {
    background: transparent;
}

#menu__toggle {
    opacity: 0;
}

.menu__btn {
    display: flex;
    /* используем flex для центрирования содержимого */
    align-items: center;
    /* центрируем содержимое кнопки */
    width: 40px;
    height: 20px;
    cursor: pointer;
    z-index: 100000000;
    color: #fff;
    position: relative;
    // transform: rotate(180deg);
}

.menu__btn>span {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before,
.menu__btn>span::after {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
    border-radius: 10px;
}

.menu__btn>span::before {
    content: '';
    top: -8px;
}

.menu__btn>span::after {
    content: '';
    top: 8px;
}

.menu__box {
    display: block;
    position: fixed;
    visibility: hidden;
    top: -100%;
    left: 0;
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 20px 0;
    list-style: none;
    text-align: left;
    background: linear-gradient(285deg, #070707 0%, #262626 100%);
    box-shadow: 1px 0px 6px rgba(0, 0, 0, .2);
}

.traparmy {
    top: -100% !important;
}

/* элементы меню */
.menu__item {
    display: block;
    padding: 12px 24px;
    color: #333;
    font-family: 'Roboto', sans-serif;
    font-size: 20px;
    font-weight: 600;
    text-decoration: none;
    z-index: 100000;
}

.menu__item:hover {
    background-color: #CFD8DC;
}

#menu__toggle:checked~.menu__btn>span {
    transform: rotate(45deg);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::before {
    top: 0;
    transform: rotate(0);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::after {
    top: 0;
    transform: rotate(90deg);
    background-color: #fff;
    border-radius: 10px;
}

#menu__toggle:checked~.menu__box {
    visibility: visible;
    top: 0;
}

.menu__btn>span,
.menu__btn>span::before,
.menu__btn>span::after {
    transition-duration: .25s;
}

.menu__box {
    transition-duration: .25s;
    z-index: 100000;
}

.menu__item {
    transition-duration: .25s;
}

.mobmenu {
    padding: 75px 40px 0;
}
</style>