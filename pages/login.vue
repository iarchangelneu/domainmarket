<template>
    <div class="login">
        <img src="@/assets/img/reg.svg" alt="">

        <div class="login__body">
            <div class="login__item">
                <h1>Авторизация</h1>

                <input type="email" name="email" id="email" v-model="username" placeholder="E-mail">
                <input type="password" name="password" id="password" v-model="password" placeholder="Пароль">


                <button class="w-100 reg">ВОЙТИ</button>

                <div class="text-center haveacc">
                    <span>Ещё нет аккаунта? <NuxtLink to="/register">Регистрация</NuxtLink></span>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            username: '',
            password: '',
            activeSwitchPosition: '50%',
        }
    },
    methods: {
        switchLeft() {
            this.activeSwitchPosition = '0%';
        },
        switchRight() {
            this.activeSwitchPosition = '50%';
        },
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Авторизация | Domain Market',
    ogTitle: 'Авторизация | Domain Market',
    description: 'Авторизация | Domain Market',
    ogDescription: 'Авторизация | Domain Market',
})
</script>
<style lang="scss" scoped>
.login {
    padding: 150px;
    height: 100vh;

    .login__body {
        display: flex;
        justify-content: center;
        margin-top: 70px;

        .login__item {
            h1 {
                font-size: 48px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--cera);
                color: #fff;
                margin-bottom: 46px;
                text-align: center;
            }

            input {
                display: block;
                width: 501px;
                margin-bottom: 20px;
                padding: 12px 23px;
                border-radius: 10px;
                border: 2px solid #FFF;
                background: transparent;

                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 110%;
                color: #fff;
                font-family: var(--cera);

                &::placeholder {
                    color: rgba(255, 255, 255, 0.60);
                }


            }

            .reg {
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
                    color: #000;
                    background: #fff;
                }
            }

            .haveacc {
                margin-top: 12px;

                span {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 110%;
                    color: #fff;
                    font-family: var(--cera);

                    a {
                        text-decoration: underline;
                        color: #fff;
                    }
                }
            }
        }
    }
}
</style>