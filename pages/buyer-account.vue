<template>
    <div class="account">
        <h1>личный кабинет</h1>

        <div class="tabs">
            <div class="t"></div>
            <div>
                <button :class="[{ active: tab == 0 }, { active: tab == 5 }, { active: tab == 6 }]" @click="tab = 0">МОИ
                    ПОКУПКИ</button>
                <button :class="{ active: tab == 2 }" @click="tab = 2">ТРАНЗАКЦИИ</button>
                <button :class="[{ active: tab == 3 }, { active: tab == 7 }]" @click="tab = 3">ОБРАТНАЯ СВЯЗЬ</button>
                <button :class="{ active: tab == 4 }" @click="tab = 4">АККАУНТ</button>
            </div>
        </div>

        <div class="tabsmob">
            <div class="mobtab">
                <button :class="[{ active: tab == 0 }, { active: tab == 5 }, { active: tab == 6 }]" @click="tab = 0">МОИ
                    ПОКУПКИ</button>
                <button :class="{ active: tab == 2 }" @click="tab = 2">ТРАНЗАКЦИИ</button>
            </div>
            <div class="mobtab">
                <button :class="[{ active: tab == 3 }, { active: tab == 7 }]" @click="tab = 3">ОБРАТНАЯ СВЯЗЬ</button>
                <button :class="{ active: tab == 4 }" @click="tab = 4">АККАУНТ</button>
            </div>

        </div>

        <div class="sales products" v-if="tab == 0">
            <div class="products__empty" v-if="buys.length <= 0">
                <h1>У ВАС ПОКА ЧТО НЕ БЫЛО ПОКУПОК</h1>
            </div>
            <div class="products__body" v-else>
                <div class="catalog__item" v-for="(buy, i) in buys" :key="buy.id">
                    <div class="name">
                        <div>
                            <small>{{ i + 1 }}.</small>
                            <p>{{ buy.products.name }}</p>
                        </div>
                        <span>{{ buy.products.price.toLocaleString() }} ₸</span>
                    </div>
                    <div class="links">
                        <button @click='createChat(buy.seller.id, buy.seller.user.first_name)'>ЧАТ С ПРОДАВЦОМ</button>
                        <!-- <NuxtLink :to="'/product/' + buy.products.id">СТРАНИЦА ТОВАРА</NuxtLink> -->
                    </div>
                </div>
            </div>
        </div>
        <div class="chats" v-if="tab == 3">
            <div class="chat__empty" v-if="chats.length <= 0">
                <h1>У ВАС ПОКА НЕ БЫЛО ЧАТОВ</h1>
            </div>
            <div class="chats__body" v-else>
                <div class="chats__item" v-for="chat in chats" :key="chat.id">
                    <div class="name">
                        <span>{{ chat.seller.user.first_name }}</span>
                        <!-- <small>23.07.2023 14:47</small> -->
                    </div>
                    <div class="text-right">
                        <button @click="openChat(chat.id, chat.seller.user.first_name)">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>

            </div>
        </div>
        <div class="account__info" v-if="tab == 4">

            <div class="info__block">
                <div>
                    <label for="email">E-mail</label>
                    <input type="email" name="email" id="email" placeholder="Ваш E-mail" v-model="email">
                </div>
                <div>
                    <label for="password">Пароль</label>
                    <input type="password" name="password" id="password" placeholder="Ваш пароль">
                </div>
            </div>

            <div class="text-center">
                <button @click="logOut()">ВЫЙТИ ИЗ АККАУНТА</button>
            </div>

        </div>
        <TheMessanger v-if="tab == 7" :chatId="chatId" :name="chatName"></TheMessanger>
        <TheTrans v-if="tab == 2" :transactions="transactions"></TheTrans>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            tab: 0,
            pathUrl: 'https://d-market.kz',
            myId: null,
            account: [],
            transactions: [],
            buys: [],
            chats: [],
            email: '',
        }
    },
    methods: {
        getAccount() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.account = response.data
                    this.myId = response.data.id
                    this.transactions = response.data.transactions
                    this.email = response.data.user.email


                })
                .catch(error => console.log(error));
        },
        openChat(chatId, chatName) {
            this.tab = 7;
            this.chatId = chatId;
            this.chatName = chatName
        },
        getBuys() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk/my-purchases`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.buys = response.data
                })
                .catch(error => {
                    console.error(error)
                })
        },
        createChat(id, name) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/messanger/new-chat`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    buyer: this.myId,
                    seller: id,
                })
                .then(response => {
                    const chatId = response.data.chat_id
                    this.openChat(chatId, name)
                })
                .catch(error => console.log(error))
        },
        getChats() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/messanger/all-chats`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(res => {
                    this.chats = res.data
                })
                .catch(error => console.log(error))
        }
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        console.log(accType)
        if (accType == 'buyer-account') {
            this.getAccount()
            this.getChats()
            this.getBuys()
        }
        else {
            window.location.href = '/login'
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Личный Кабинет | Domain Market',
    ogTitle: 'Личный Кабинет | Domain Market',
    description: 'Личный Кабинет | Domain Market',
    ogDescription: 'Личный Кабинет | Domain Market',
})
</script>

<style lang="scss" scoped>
.chat__empty {
    margin-top: 100px;
    margin-bottom: 100px;

    h1 {
        font-size: 24px !important;
        font-style: normal;
        font-weight: 400;
        line-height: 150%;
        color: #fff;
        font-family: var(--cera);
        text-align: center;
    }
}

.account__info {
    margin-top: 150px;
    width: 100%;

    @media (max-width: 1024px) {
        margin-top: 40px;
    }

    button {
        margin-top: 36px;
        border-radius: 10px;
        border: 3px solid #FFF;
        background: transparent;
        padding: 12px 48px;

        font-size: 18px;
        font-style: normal;
        font-weight: 400;
        line-height: 110%;
        font-family: var(--cera);
        color: #fff;
        transition: all .3s ease;

        &:hover {
            color: #000;
            background: #fff;
        }
    }

    .info__block {
        width: 100%;
        display: flex;
        justify-content: center;
        gap: 20px;

        @media (max-width: 1024px) {
            flex-direction: column;
        }

        label {
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
            font-family: var(--cera);
            color: #fff;
            margin-bottom: 10px;
            margin-left: 20px;
            display: block;
        }

        input {
            width: 29.167vw;
            border-radius: 10px;
            border: 3px solid #FFF;
            font-family: var(--cera);
            color: #fff;
            background: transparent;
            padding: 12px 20px;
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;

            @media (max-width: 1024px) {
                width: 100%;
            }

        }
    }
}

.chats {
    margin-top: 36px;

    .chats__body {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 24px;

        .chats__item {
            border-radius: 10px;
            border: 2px solid #FFF;
            padding: 36px;

            @media (max-width: 1024px) {
                padding: 20px;
                width: 100%;
            }

            .name {
                display: flex;
                gap: 0 42px;
                align-items: center;

                @media (max-width: 1024px) {
                    flex-direction: column;
                    gap: 10px 0;
                    align-items: flex-start;
                }

                span {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    color: #fff;
                    font-family: var(--cera);

                    @media (max-width: 1024px) {
                        font-size: 20px;
                    }
                }

                small {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    color: #fff;
                    font-family: var(--cera);

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }

            button {
                margin-top: 36px;
                background: transparent;
                padding: 12px 27px;
                border-radius: 10px;
                border: 2px solid #FFF;

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
        }
    }
}

.products {
    margin-top: 36px;

    .products__empty {
        margin-top: 100px;
        margin-bottom: 100px;
        display: flex;
        justify-content: center;

        h1 {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 150%;
            font-family: var(--cera);
            color: #fff;
            max-width: 636px;
            text-align: center;
        }
    }

    .products__body {
        margin-top: 40px;
        display: flex;
        flex-wrap: wrap;
        gap: 36px 95px;

        @media (max-width: 1440px) {
            justify-content: center;
        }

        .catalog__item {
            width: 476px;

            @media (max-width: 1100px) {
                width: 100%;
            }

            .name {
                display: flex;
                justify-content: space-between;
                align-items: center;

                div {
                    display: flex;
                    gap: 24px;

                    @media (max-width: 1024px) {
                        gap: 10px;
                    }

                    small {
                        font-size: 18px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        text-transform: uppercase;
                        font-family: var(--cera);
                        color: #fff;
                    }

                    p {
                        margin-bottom: 0;
                        font-size: 18px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        text-transform: uppercase;
                        font-family: var(--cera);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }
                }

                span {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--cera);
                    color: #fff;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                        white-space: nowrap;
                    }
                }
            }

            .links {
                display: flex;
                gap: 20px;
                margin-top: 28px;

                @media (max-width: 1024px) {
                    margin-top: 22px;
                }

                button,
                a {
                    flex: 1;
                    border-radius: 10px;
                    border: 2px solid #FFF;
                    background: transparent;
                    padding: 12px 28px;

                    font-size: 18px;
                    font-style: normal;
                    white-space: nowrap;
                    font-weight: 400;
                    line-height: 110%;
                    font-family: var(--cera);
                    color: #fff;
                    text-decoration: none;
                    transition: all .3s ease;

                    &:hover {
                        color: #000;
                        background: #fff;
                    }

                    @media (max-width: 1024px) {
                        font-size: 16px;
                        padding: 10px 18px;
                    }
                }
            }
        }
    }

    .products__filter {
        justify-content: flex-end;
        display: flex;
        gap: 20px;

        button {
            padding: 12px 24px;
            background: transparent;
            border-radius: 10px;
            border: 2px solid #FFF;
            display: flex;
            gap: 0 10px;
            align-items: center;

            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
            color: #FFF;
            font-family: var(--cera);
            transition: all .3s ease;

            &:nth-child(1):hover {
                color: #000;
                background: #fff;
            }
        }
    }
}

.active {
    background: #fff !important;
    color: #000 !important;
}

.account {
    padding: 120px 150px 72px;

    @media(max-width: 1500px) {
        padding: 120px 50px 50px;
    }

    @media(max-width: 1024px) {
        padding: 120px 20px 50px;
    }


    h1 {
        font-size: 48px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        text-transform: uppercase;
        font-family: var(--cera);
        color: #fff;

        @media (max-width: 1024px) {
            font-size: 32px;
        }
    }

    .tabsmob {
        display: none;
        margin-top: 20px;

        @media (max-width: 1024px) {
            display: block;
        }

        .mobtab {
            display: flex;
            margin-bottom: 10px;


            button {
                &:nth-child(1) {
                    border-top-right-radius: 0;
                    border-bottom-right-radius: 0;
                }

                &:nth-child(2) {
                    border-left: 0;
                    border-top-left-radius: 0;
                    border-bottom-left-radius: 0;
                }
            }
        }

        button {
            border-radius: 10px;
            border: 2px solid #FFF;
            background: transparent;
            padding: 13px 17px;
            color: #fff;
            flex: 1;
        }
    }

    .tabs {
        display: flex;
        justify-content: space-between;
        margin-top: 36px;

        @media (max-width: 1024px) {
            display: none;
        }

        .t {
            width: 60%;

            @media(max-width: 1500px) {
                width: 30%;
            }

            @media (max-width: 1024px) {
                display: none;
            }
        }

        div {
            display: flex;
            width: 100%;

            button {
                flex: 1;
                padding: 12px 12px;
                background: transparent;
                border: 2px solid #fff;

                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 110%;
                font-family: var(--cera);
                color: #fff;
                transition: all .3s ease;

                &:nth-child(1) {
                    border-top-left-radius: 10px;
                    border-bottom-left-radius: 10px;
                    border-right: 0;
                }

                &:nth-child(2) {
                    border-right: 0;
                }



                &:last-child {
                    border-top-right-radius: 10px;
                    border-bottom-right-radius: 10px;
                    border-left: 0;
                }
            }
        }

    }
}
</style>