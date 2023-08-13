<template>
    <div class="account">
        <h1>личный кабинет</h1>

        <div class="tabs">
            <div class="t"></div>
            <div>
                <button :class="[{ active: tab == 0 }, { active: tab == 5 }, { active: tab == 6 }]" @click="tab = 0">МОИ
                    ТОВАРЫ</button>
                <button :class="{ active: tab == 1 }" @click="tab = 1">МОИ ПРОДАЖИ</button>
                <button :class="{ active: tab == 2 }" @click="tab = 2">ТРАНЗАКЦИИ</button>
                <button :class="[{ active: tab == 3 }, { active: tab == 7 }]" @click="tab = 3">ОБРАТНАЯ СВЯЗЬ</button>
                <button :class="{ active: tab == 4 }" @click="tab = 4">АККАУНТ</button>
            </div>
        </div>

        <div class="tabsmob">
            <div class="mobtab">
                <button :class="[{ active: tab == 0 }, { active: tab == 5 }, { active: tab == 6 }]" @click="tab = 0">МОИ
                    ТОВАРЫ</button>
                <button :class="{ active: tab == 1 }" @click="tab = 1">МОИ ПРОДАЖИ</button>
            </div>
            <div class="mobtab">
                <button :class="{ active: tab == 2 }" @click="tab = 2">ТРАНЗАКЦИИ</button>
                <button :class="[{ active: tab == 3 }, { active: tab == 7 }]" @click="tab = 3">ОБРАТНАЯ СВЯЗЬ</button>
            </div>
            <button class="w-100" :class="{ active: tab == 4 }" @click="tab = 4">
                АККАУНТ
            </button>
        </div>


        <div class="products" v-if="tab == 0">
            <div class="products__filter">
                <button @click="tab = 5">ДОБАВИТЬ ТОВАР</button>
            </div>
            <div class="products__empty" v-if="products.length <= 0">
                <h1>У ВАС ПОКА ЧТО НЕ ЗАГРУЖЕНО НИ ОДНОГО ТОВАРА.
                    ДОБАВЬТЕ ТОВАР И НАЧНИТЕ ЗАРАБАТЫВАТЬ!</h1>
            </div>
            <div class="products__body" v-else>
                <div class="catalog__item" v-for="(product, i) in products" :key="product.i">
                    <div class="name">
                        <div>
                            <small>{{ i + 1 }}.</small>
                            <p>{{ product.name }}</p>
                        </div>
                        <span>{{ product.price.toLocaleString() }} ₸</span>
                    </div>
                    <div class="links">
                        <button @click='openEditTab(product.id)'>ИЗМЕНИТЬ</button>
                        <NuxtLink :to="'/product/' + product.id">СТРАНИЦА ТОВАРА</NuxtLink>
                    </div>
                </div>
            </div>
        </div>

        <div class="sales products" v-if="tab == 1">
            <div class="products__empty" v-if="sales.length <= 0">
                <h1>У ВАС ПОКА ЧТО НЕ БЫЛО ПРОДАЖ</h1>
            </div>
            <div class="products__body" v-else>
                <div class="catalog__item" v-for="(sale, i) in sales" :key="sale.id">
                    <div class="name">
                        <div>
                            <small>{{ i + 1 }}.</small>
                            <p>{{ sale.products.name }}</p>
                        </div>
                        <span>{{ sale.products.price.toLocaleString() }} ₸</span>
                    </div>
                    <div class="links">
                        <button @click='createChat(sale.buyer.id, sale.buyer.user.email)'>ЧАТ С ПОКУПАТЕЛЕМ</button>
                        <NuxtLink to="/product">СТРАНИЦА ТОВАРА</NuxtLink>
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
                        <span>{{ chat.buyer.user.email }}</span>
                        <!-- <small>23.07.2023 14:47</small> -->
                    </div>
                    <div class="text-right">
                        <button @click="openChat(chat.id, chat.buyer.user.email)">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>

            </div>
        </div>
        <div class="account__info" v-if="tab == 4">
            <div class="info__body">
                <div class="left__side">
                    <label for="name">Отображаемое имя</label>
                    <div class="name">
                        <input type="text" id="name" placeholder="Ваше Имя" v-model="name" disabled ref="name">
                        <img src="@/assets/img/pen.svg" style="cursor: pointer;" alt="" @click="unlockEdit($refs.name)">
                    </div>

                    <label for="email">E-mail</label>
                    <input type="email" name="email" id="email" v-model="email" placeholder="Ваш E-mail">

                    <label for="password">Пароль</label>
                    <input type="password" name="password" id="password" v-model="password" placeholder="Ваш Пароль">
                </div>
                <div class="right__side">
                    <label for="description">Описание профиля</label>
                    <div class="desc">
                        <textarea name="description" id="description" v-model="description" cols="30" rows="9" ref="desc"
                            disabled></textarea>
                        <img src="@/assets/img/pen.svg" style="cursor: pointer;" @click="unlockEdit($refs.desc)" alt="">
                    </div>
                </div>
            </div>

            <div class="btns">
                <button @click="editLk()" ref="editBtn">СОХРАНИТЬ ИЗМЕНЕНИЯ</button>
                <button @click="logOut()">ВЫЙТИ ИЗ АККАУНТА</button>
            </div>
        </div>
        <TheCreate v-if="tab == 5"></TheCreate>
        <TheEdit v-if="tab == 6" :productId="sendId"></TheEdit>
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
            tab: 4,
            name: '',
            email: '',
            description: '',
            password: '',
            first_name: '',
            chats: [],
            sales: [],
            products: [],
            transactions: [],
            pathUrl: 'https://d-market.kz',
            account: [],
            sendId: null,
            chatId: null,
            myId: null,
        }
    },
    methods: {
        unlockEdit(ref) {
            ref.disabled = false
            ref.focus()
        },
        formatDate(dateString) {
            const date = new Date(dateString);
            const formattedDate = `${date.getDate().toString().padStart(2, '0')}.${(date.getMonth() + 1).toString().padStart(2, '0')}.${date.getFullYear()}`;
            return formattedDate;
        },
        openEditTab(productId) {
            this.tab = 6;
            this.sendId = productId;
        },
        getAccount() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/seller/seller-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.account = response.data
                    this.description = response.data.description
                    this.email = response.data.user.email
                    this.name = response.data.user.first_name
                    this.sales = response.data.my_sales
                    this.products = response.data.products
                    this.transactions = response.data.transactions
                    this.myId = response.data.id

                })
                .catch(error => console.log(error));
        },
        editLk() {
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/seller/seller-lk/edit/`

            this.$refs.editBtn.innerHTML = 'Сохраняем'
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .put(path,
                    {
                        user: {
                            first_name: this.name,
                            email: this.email,
                        },
                        description: this.description,
                    }
                )
                .then((res) => {
                    console.log(res)
                    if (res.status == 200) {
                        this.name = res.data.user.first_name
                        this.description = res.data.description
                        this.email = res.data.user.email

                        this.$refs.editBtn.innerHTML = 'Успешно сохранено'
                    }
                })
                .catch((error) => {
                    console.error(error);
                    this.$refs.editBtn.innerHTML = 'Ошибка'
                });
        },
        getChats() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/messanger/all-chats`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.chats = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        },
        openChat(chatId, chatName) {
            this.tab = 7;
            this.chatId = chatId;
            this.chatName = chatName
        },
        createChat(id, name) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/messanger/new-chat`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    buyer: id,
                    seller: this.myId,
                })
                .then(response => {
                    const chatId = response.data.chat_id
                    this.openChat(chatId, name)
                })
                .catch(error => console.log(error))
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        console.log(accType)
        if (accType == 'seller-account') {
            this.getAccount()
            this.getChats()
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
    margin-top: 36px;

    .btns {
        display: flex;
        justify-content: center;
        gap: 20px;
        margin-top: 36px;

        @media (max-width: 1024px) {
            flex-direction: column;
            justify-content: end;
            align-items: flex-end;
        }

        button {
            border-radius: 10px;
            border: 2px solid #FFF;
            background: transparent;
            padding: 12px 38px;

            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
            font-family: var(--cera);
            color: #fff;
            transition: all .3s ease;

            @media (max-width: 1024px) {
                padding: 12px 19px;
                font-size: 16px;

            }

            &:hover {
                color: #000;
                background: #fff;
            }
        }
    }

    .info__body {
        display: flex;
        gap: 40px;

        @media (max-width: 1024px) {
            flex-direction: column;
        }

        label {
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            color: #fff;
            font-family: var(--cera);
            line-height: 110%;
            text-transform: capitalize;
            margin-bottom: 10px;
            margin-left: 20px;
            display: block;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }

        input {
            padding: 12px 20px;
            border-radius: 10px;
            border: 2px solid #FFF;
            background: transparent;
            display: block;
            width: 100%;

            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
            font-family: var(--cera);
            color: #fff;
            margin-bottom: 20px;

            @media (max-width: 1024px) {
                font-size: 16px;
            }

            &:last-child {
                margin-bottom: 0;
            }

            &::placeholder {
                color: rgba(255, 255, 255, 0.60);
            }
        }

        textarea {
            padding: 17px 20px;
            border-radius: 10px;
            border: 2px solid #FFF;
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            color: #fff;
            font-family: var(--cera);
            background: transparent;
            width: 100%;
        }

        .right__side {
            width: 100%;

            .desc {
                position: relative;

                img {
                    position: absolute;
                    top: 5%;
                    right: 2%;
                }
            }
        }

        .left__side {
            width: 100%;

            .name {
                position: relative;

                img {
                    position: absolute;
                    top: 20%;
                    right: 2%;
                }
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

            .text-right {
                text-align: left !important;
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

                @media (max-width: 1024px) {
                    font-size: 16px;
                }

                &:hover {
                    color: #000;
                    background: #fff;
                }
            }
        }
    }
}

.sales {
    .products__body {
        .catalog__item {
            .links {

                button,
                a {

                    @media (max-width: 1024px) {
                        font-size: 12px !important;
                        text-align: center;
                    }
                }
            }
        }
    }
}

.products {
    margin-top: 36px;

    .products__empty {
        margin-top: 100px;
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
    height: 100vh;

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

                &:nth-child(2),
                &:nth-child(3) {
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