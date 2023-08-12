<template>
    <div class="login">
        <img src="@/assets/img/reg.svg" alt="">

        <div class="login__body">
            <div class="login__item">
                <h1>Авторизация</h1>

                <input type="email" name="email" id="email" v-model="username" placeholder="E-mail">
                <input type="password" name="password" id="password" v-model="password" placeholder="Пароль">


                <button class="w-100 reg" @click="login()">ВОЙТИ</button>

                <div class="text-center haveacc">
                    <span>Ещё нет аккаунта? <NuxtLink to="/register">Регистрация</NuxtLink></span>
                </div>
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
            username: '',
            password: '',
            pathUrl: 'https://d-market.kz',
        }
    },
    methods: {
        login() {
            const path = `${this.pathUrl}/api/main/authorization`
            // const csrf = this.getCSRFToken()

            // axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, { username: this.username, password: this.password })
                .then((res) => {

                    if (res.status == 202) {

                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        localStorage.setItem('accountType', res.data.redirect_url)
                        window.location.href = res.data.redirect_url
                    }
                    else {

                    }
                    console.log(res)
                })
                .catch((error) => {
                    console.log(error);
                    // this.error = error.response.data.non_field_errors[0]
                });
        }
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