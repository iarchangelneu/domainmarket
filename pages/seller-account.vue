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


        <div class="products" v-if="tab == 0">
            <div class="products__filter">
                <button @click="tab = 5">ДОБАВИТЬ ТОВАР</button>
                <button>
                    <span>ФИЛЬТРЫ</span>
                    <img src="@/assets/img/filtersarrow.svg" alt="">
                </button>
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
                        <button @click="tab = 6">ИЗМЕНИТЬ</button>
                        <NuxtLink to="/product">СТРАНИЦА ТОВАРА</NuxtLink>
                    </div>
                </div>
            </div>
        </div>

        <div class="sales products" v-if="tab == 1">
            <div class="products__empty" v-if="products.length <= 0">
                <h1>У ВАС ПОКА ЧТО НЕ БЫЛО ПРОДАЖ</h1>
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
                        <button @click="tab = 7">ЧАТ С ПОКУПАТЕЛЕМ</button>
                        <NuxtLink to="/product">СТРАНИЦА ТОВАРА</NuxtLink>
                    </div>
                </div>
            </div>
        </div>
        <div class="chats" v-if="tab == 3">
            <div class="chats__body">
                <div class="chats__item">
                    <div class="name">
                        <span>alex.ivanov@gmail.com</span>
                        <small>23.07.2023 14:47</small>
                    </div>
                    <div class="text-right">
                        <button @click="tab = 7">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>
                <div class="chats__item">
                    <div class="name">
                        <span>alex.ivanov@gmail.com</span>
                        <small>23.07.2023 14:47</small>
                    </div>
                    <div class="text-right">
                        <button @click="tab = 7">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>
                <div class="chats__item">
                    <div class="name">
                        <span>alex.ivanov@gmail.com</span>
                        <small>23.07.2023 14:47</small>
                    </div>
                    <div class="text-right">
                        <button @click="tab = 7">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>
                <div class="chats__item">
                    <div class="name">
                        <span>alex.ivanov@gmail.com</span>
                        <small>23.07.2023 14:47</small>
                    </div>
                    <div class="text-right">
                        <button @click="tab = 7">ОТКРЫТЬ ЧАТ</button>
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
                        <textarea name="description" id="description" cols="30" rows="9" ref="desc" disabled></textarea>
                        <img src="@/assets/img/pen.svg" style="cursor: pointer;" @click="unlockEdit($refs.desc)" alt="">
                    </div>
                </div>
            </div>

            <div class="btns">
                <button>СОХРАНИТЬ ИЗМЕНЕНИЯ</button>
                <button>ВЫЙТИ ИЗ АККАУНТА</button>
            </div>
        </div>
        <TheCreate v-if="tab == 5"></TheCreate>
        <TheEdit v-if="tab == 6"></TheEdit>
        <TheMessanger v-if="tab == 7"></TheMessanger>
        <TheTrans v-if="tab == 2"></TheTrans>
    </div>
</template>
<script>
export default {
    data() {
        return {
            tab: 4,
            name: '',
            email: '',
            description: '',
            password: '',
            products: [
                {
                    name: 'topdomain.su',
                    price: 11540
                },
                {
                    name: 'topdomain.su',
                    price: 11540
                },
                {
                    name: 'topdomain.su',
                    price: 11540
                },
                {
                    name: 'topdomain.su',
                    price: 11540
                },
                {
                    name: 'topdomain.su',
                    price: 11540
                },
                {
                    name: 'topdomain.su',
                    price: 11540
                },
                {
                    name: 'topdomain.su',
                    price: 11540
                },
            ]
        }
    },
    methods: {
        unlockEdit(ref) {
            ref.disabled = false
            ref.focus()
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
.account__info {
    margin-top: 36px;

    .btns {
        display: flex;
        justify-content: center;
        gap: 20px;
        margin-top: 36px;

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

            &:hover {
                color: #000;
                background: #fff;
            }
        }
    }

    .info__body {
        display: flex;
        gap: 40px;

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

            .name {
                display: flex;
                gap: 0 42px;
                align-items: center;

                span {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    color: #fff;
                    font-family: var(--cera);
                }

                small {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    color: #fff;
                    font-family: var(--cera);
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

        .catalog__item {
            width: 476px;

            .name {
                display: flex;
                justify-content: space-between;
                align-items: center;

                div {
                    display: flex;
                    gap: 24px;

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
                }
            }

            .links {
                display: flex;
                gap: 20px;
                margin-top: 28px;

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

    h1 {
        font-size: 48px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        text-transform: uppercase;
        font-family: var(--cera);
        color: #fff;
    }

    .tabs {
        display: flex;
        justify-content: space-between;
        margin-top: 36px;

        .t {
            width: 60%;
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