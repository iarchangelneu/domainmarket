<template>
    <div class="register">
        <img src="@/assets/img/reg.svg" class="pcimg" alt="">

        <div class="register__body">
            <div class="register__item">
                <h1>регистрация</h1>

                <input type="email" name="email" id="email" v-model="username" placeholder="E-mail">
                <input type="password" name="password" id="password" v-model="password" placeholder="Пароль">
                <input type="password" name="repeat_password" id="repeat_password" placeholder="Повторите пароль">

                <div class="switch-button">
                    <span class="active" :style="{ left: activeSwitchPosition }"></span>
                    <button type="button" class="switch-button-case left"
                        :class="{ 'active-case': activeSwitchPosition === '0%' }" @click="switchLeft">ПОКУПАТЕЛЬ</button>
                    <button type="button" class="switch-button-case right"
                        :class="{ 'active-case': activeSwitchPosition === '50%' }" @click="switchRight">ПРОДАВЕЦ</button>
                </div>

                <label class="custom-checkbox">
                    <input type="checkbox" class="" value="0">
                    <p class="checkbox-text m-0">Принимаю условия <NuxtLink to='/terms'>Пользовательского соглашения
                        </NuxtLink> и<br>
                        <NuxtLink to='/polytics'>Политики
                            конфиденциальности</NuxtLink>
                    </p>
                </label>

                <button class="w-100 reg" @click="register()">ЗАРЕГИСТРИРОВАТЬСЯ</button>

                <div class="text-center haveacc">
                    <span>Уже есть аккаунт? <NuxtLink to="/login">Вход</NuxtLink></span>
                </div>
            </div>
        </div>

        <div class="text-center">
            <img src="@/assets/img/reg.svg" class="mobimg img-fluid" alt="">
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
            activeSwitchPosition: '50%',
            pathUrl: 'https://d-market.kz',
        }
    },
    methods: {
        switchLeft() {
            this.activeSwitchPosition = '0%';
        },
        switchRight() {
            this.activeSwitchPosition = '50%';
        },
        register() {
            const buyer = `${this.pathUrl}/api/main/registration/buyer`
            const seller = `${this.pathUrl}/api/main/registration/seller`
            if (this.activeSwitchPosition == '50%') {
                axios
                    .post(seller, { email: this.username, password: this.password, username: this.username })
                    .then((res) => {
                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        console.log(res)
                        localStorage.setItem('accountType', res.data.redirect_url)
                        window.location.href = res.data.redirect_url
                    })
                    .catch((error) => {
                        console.error(error);
                    });
            }
            else {
                axios
                    .post(buyer, { email: this.username, password: this.password, username: this.username })
                    .then((res) => {
                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        console.log(res)
                        localStorage.setItem('accountType', res.data.redirect_url)
                        window.location.href = res.data.redirect_url
                    })
                    .catch((error) => {
                        console.error(error);
                    });
            }
        },
    }
}
</script >
<script setup>
useSeoMeta({
    title: 'Регистрация | Domain Market',
    ogTitle: 'Регистрация | Domain Market',
    description: 'Регистрация | Domain Market',
    ogDescription: 'Регистрация | Domain Market',
})
</script>
<style lang="scss" scoped>
.register {
    padding: 150px;
    height: 100vh;

    @media (max-width: 1024px) {
        padding: 20px;
    }

    .mobimg {
        display: none;
    }

    @media (max-width: 1024px) {
        .pcimg {
            display: none;
        }

        .mobimg {
            display: block;
            margin-top: 50px;
            width: 100%;
        }
    }

    .register__body {
        display: flex;
        justify-content: center;
        margin-top: 70px;

        .register__item {
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

                @media (max-width: 1024px) {
                    font-size: 32px;
                    margin-bottom: 35px;
                }
            }

            input {
                display: block;
                width: 100%;
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

                @media (max-width: 1024px) {
                    font-size: 16px;
                }

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

.custom-checkbox input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    border-radius: 4px;
}

.custom-checkbox .checkbox-text:before {
    content: url('@/assets/img/check.svg');
    display: inline-block;
    vertical-align: middle;
    width: 20px;
    height: 20px;
    margin-right: 12px;
    border: 1px solid #fff;
    border-radius: 4px;
    margin-bottom: 4px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
    content: url('@/assets/img/check.svg');
    font-size: 16px;
    color: black;
    text-align: center;
    line-height: 20px;
    background: #fff;
}

.custom-checkbox a {
    color: #fff;
    text-decoration: underline !important;
    z-index: 50;
}

.custom-checkbox p {
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--cera);
    color: #fff;
}

.custom-checkbox {
    margin-bottom: 30px;
}

.switch-button {
    width: 100%;
    height: 47.8px;
    text-align: center;
    position: relative;
    left: 50%;
    -webkit-transform: translate3D(-50%, -50%, 0);
    transform: translate3D(-50%, -50%, 0);
    will-change: transform;
    z-index: 197 !important;
    cursor: pointer;
    transition: .3s ease all;
    border: 2px solid #fff;
    background: transparent;
    border-radius: 8px;
    margin-top: 50px;
}

.switch-button-case {
    display: inline-block;
    background: none;
    width: 50%;
    height: 100%;
    color: #36589f;
    position: relative;
    border: none;
    transition: .3s ease all;

    padding-bottom: 1px;

    font-style: normal;
    font-weight: 400;
    font-size: 18px;
    line-height: 110%;
    text-transform: uppercase;
    color: #fff;
    font-family: var(--cera);
}

.switch-button-case:hover {
    color: #fff;
    cursor: pointer;
}

.switch-button-case:focus {
    outline: none;
}

.switch-button .active {
    color: #000;
    background-color: #fff;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.1), 0px 1px 2px rgba(0, 0, 0, 0.06);
    border-radius: 6px;
    position: absolute;
    left: 0;
    top: 0;
    width: 50%;
    height: 100%;

    z-index: -1 !important;
    margin-top: 0;
    margin-left: 0;
    transition: .3s ease-out all;
}

.switch-button .active-case {
    color: #000;
}
</style>