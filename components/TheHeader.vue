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
            <img src="@/assets/img/cart.svg" style="cursor: pointer;" @click="cart = !cart" alt="">
            <div class="cart" :class="{ active: cart }">

                <div class="name w-100 d-flex justify-content-between align-items-center">
                    <h3>корзина</h3>
                    <img src="@/assets/img/close.svg" style="cursor: pointer;" @click="cart = false" alt="">
                </div>

                <div class="tru d-flex align-items-center justify-content-between">
                    <span>EKOTOURKAZAKHSTAN.COM</span>
                    <div>
                        <small>
                            11 540 ₸
                        </small>
                        <img src="@/assets/img/mkn.svg" alt="">
                    </div>
                </div>
            </div>
            <div v-if="testLogin">
                <NuxtLink data-toggle="modal" data-target="#inModal">230 000 ₸</NuxtLink>
                <NuxtLink to="/seller-account">Личный кабинет</NuxtLink>
            </div>
            <div v-else>
                <NuxtLink to="/">Вход</NuxtLink>
                <NuxtLink to="/">Регистрация</NuxtLink>
            </div>
        </div>

    </header>

    <TheOutModal></TheOutModal>
    <TheInModal></TheInModal>
</template>
<script>
export default {
    data() {
        return {
            hideHeaderOnPages: ['login', 'register'],
            testLogin: true,
            cart: false,
        }
    }
}
</script>
<style lang="scss" scoped>
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