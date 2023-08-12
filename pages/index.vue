<template>
    <div class="home">
        <div class="main d-flex align-items-center justify-content-between">
            <div>
                <h2>
                    Найдите имя
                </h2>
                <h1>ДЛЯ СВОЕГО
                    <span>ВЕБ-САЙТА</span>
                    <img src="@/assets/img/mainline.svg" alt="">
                </h1>

                <div class="search">
                    <p>Доменные имена помогают пользователям найти то, что им нужно. Найдите то, что идеально представит
                        Вашу уникальность.</p>

                    <div class="d-flex">
                        <div class="input-group">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/lupa.svg"
                                        alt=""></span>
                            </div>
                            <input type="text" class="form-control" placeholder="" aria-label="search"
                                aria-describedby="basic-addon1">
                        </div>
                        <button>НАЙТИ</button>
                    </div>
                </div>
            </div>

            <div class="parallax" ref="scene">
                <img src="@/assets/img/main.png" alt="" data-title="no.1" data-depth="0.1">
                <span data-title="no.2" data-depth="0.85">.com</span>
                <span data-title="no.3" data-depth="0.55">.su</span>
                <span data-title="no.4" data-depth="0.45">.asia</span>
                <span data-title="no.5" data-depth="0.15">.kz</span>
                <span data-title="no.6" data-depth="0.25">.net</span>
                <span data-title="no.7" data-depth="0.85">.me</span>
                <span data-title="no.8" data-depth="0.35">.shop</span>
            </div>
        </div>

        <div class="catalog">
            <div class="trap">
                <h1>Каталог</h1>
                <NuxtLink to="/catalog">СМОТРЕТЬ ВСЕ</NuxtLink>
            </div>

            <div class="catalog__body">

                <NuxtLink to="/" class="catalog__item" v-for="product in products" :key="product.name">
                    <p class="mb-0">
                        {{ product.name }}
                    </p>
                    <img src="@/assets/img/homearrow.svg" alt="">
                </NuxtLink>


            </div>
        </div>

        <div class="safety">
            <h1>Безопасность</h1>

            <div>
                <img src="@/assets/img/safety.png" alt="">
                <div>
                    <p class="mb-0">Ваша безопасность - наш приоритет! Мы серьезно относимся к защите ваших сделок на нашей
                        платформе. Мы
                        предлагаем использование системы “Безопасная сделка” для оплаты и выполнения работ. Деньги
                        покупателя
                        хранятся на счете в системе до получения товара, обеспечивая защиту как для покупателя, так и для
                        продавца. Мы стремимся обеспечить надежную среду для всех пользователей нашей платформы и создать
                        лучшие
                        условия для покупки и продажи доменов.</p>

                    <div>
                        <NuxtLink to="/catalog">ПЕРЕЙТИ К ПОКУПКАМ</NuxtLink>
                    </div>
                </div>
            </div>
        </div>
        <div class="reviews">
            <h1>ОТЗЫВЫ ПОЛЬЗОВАТЕЛЕЙ</h1>

            <swiper :slidesPerView="2" :spaceBetween="30" :centeredSlides="true" :loop="true" :pagination="pagination"
                :navigation="navigation" :modules="modules" class="mySwiper">
                <swiper-slide>
                    <div class="slide">
                        <div class="name d-flex justify-content-between align-items-center">
                            <span>Айгуль Жумаисова</span>
                            <small>12.08.2023</small>
                        </div>

                        <p>Отличное внимание к деталям и отзывчивая поддержка - это то, что делает D-Market стоящей
                            платформой для тех, кто ищет удобный способ управления доменами.</p>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="slide">
                        <div class="name d-flex justify-content-between align-items-center">
                            <span>Анна Соколова</span>
                            <small>1.08.2023</small>
                        </div>

                        <p>Биржа доменов D-Market оказалась настоящим открытием для меня. Легкий в использовании интерфейс,
                            широкий выбор доменов и оперативная поддержка клиентов сделали процесс покупки домена
                            удивительно простым. Рекомендую D-Market всем, кто ищет надежное и эффективное место для
                            торговли доменами.</p>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="slide">
                        <div class="name d-flex justify-content-between align-items-center">
                            <span>Айгуль Жумаисова</span>
                            <small>12.08.2023</small>
                        </div>

                        <p>Отличное внимание к деталям и отзывчивая поддержка - это то, что делает D-Market стоящей
                            платформой для тех, кто ищет удобный способ управления доменами.</p>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="slide">
                        <div class="name d-flex justify-content-between align-items-center">
                            <span>Анна Соколова</span>
                            <small>1.08.2023</small>
                        </div>

                        <p>Биржа доменов D-Market оказалась настоящим открытием для меня. Легкий в использовании интерфейс,
                            широкий выбор доменов и оперативная поддержка клиентов сделали процесс покупки домена
                            удивительно простым. Рекомендую D-Market всем, кто ищет надежное и эффективное место для
                            торговли доменами.</p>
                    </div>
                </swiper-slide>

                <img src="@/assets/img/prev.svg" class="prev" style="cursor: pointer;" alt="">
                <img src="@/assets/img/next.svg" class="next" style="cursor: pointer;" alt="">
            </swiper>
            <div class="sales-pagination"></div>
        </div>

        <div class="faq">
            <h1>частые вопросы</h1>

            <div class="d-flex justify-content-end">
                <div class="faq__body">
                    <div class="faq__item">
                        <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample0"
                            aria-expanded="false" aria-controls="collapseExample0" id="faqbtn0">
                            <span>Что такое домен и почему он важен? </span>
                            <img src="@/assets/img/faqarrow.svg" alt="" id="arrow0">
                        </button>
                        <div class="collapse" id="collapseExample0">
                            <div class="card card-body">
                                <p>Домен - это уникальный веб-адрес, который позволяет пользователям находить ваш сайт в
                                    Интернете. Он служит визитной карточкой вашей онлайн-идентичности и помогает создать
                                    узнаваемый и профессиональный облик.
                                </p>

                            </div>
                        </div>
                    </div>
                    <div class="faq__item">
                        <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample1"
                            aria-expanded="false" aria-controls="collapseExample1" id="faqbtn1">
                            <span>Как я могу купить домен? </span>
                            <img src="@/assets/img/faqarrow.svg" alt="" id="arrow1">
                        </button>
                        <div class="collapse" id="collapseExample1">
                            <div class="card card-body">
                                <p>Чтобы приобрести домен, зарегистрируйтесь на Domain Market в качестве покупателя. В
                                    каталоге найдите интересующий вас вариант домена, нажмите на него, чтобы увидеть
                                    дополнительную информацию, а затем следуйте инструкциям по покупке.
                                </p>

                            </div>
                        </div>
                    </div>
                    <div class="faq__item">
                        <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample2"
                            aria-expanded="false" aria-controls="collapseExample2" id="faqbtn2">
                            <span>Почему стоимость доменов так сильно отличается? </span>
                            <img src="@/assets/img/faqarrow.svg" alt="" id="arrow2">
                        </button>
                        <div class="collapse" id="collapseExample2">
                            <div class="card card-body">
                                <p>Стоимость домена зависит от многих факторов, включая его длину, ключевые слова,
                                    уникальность, рыночную конкуренцию и потенциал использования.
                                </p>

                            </div>
                        </div>
                    </div>
                    <div class="faq__item">
                        <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample3"
                            aria-expanded="false" aria-controls="collapseExample3" id="faqbtn3">
                            <span>Какая защита предоставляется при покупке домена? </span>
                            <img src="@/assets/img/faqarrow.svg" alt="" id="arrow3">
                        </button>
                        <div class="collapse" id="collapseExample3">
                            <div class="card card-body">
                                <p>Мы обеспечиваем безопасную сделку через нашу биржу. Все домены проверяются на наличие
                                    задолженностей и юридических ограничений перед их продажей.
                                </p>

                            </div>
                        </div>
                    </div>
                    <div class="faq__item">
                        <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample4"
                            aria-expanded="false" aria-controls="collapseExample4" id="faqbtn4">
                            <span>Как я могу связаться с продавцом/покупателем домена? </span>
                            <img src="@/assets/img/faqarrow.svg" alt="" id="arrow4">
                        </button>
                        <div class="collapse" id="collapseExample4">
                            <div class="card card-body">
                                <p>После совершения покупки в Вашем Личном кабинете будет доступен чат с
                                    продавцом/покупателем.
                                </p>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/pagination';
import { Pagination, Navigation } from 'swiper/modules';

import Parallax from 'parallax-js';
export default {
    components: {
        Swiper,
        SwiperSlide,
    },
    data() {
        return {
            modules: [Pagination, Navigation],
            pagination: {
                el: '.sales-pagination',
                clickable: true,
            },
            navigation: {
                prevEl: '.prev',
                nextEl: '.next',
            },
            products: [
                {
                    name: 'topdomain.kz'
                },
                {
                    name: 'topdomain.kz'
                },
                {
                    name: 'topdomain.kz'
                },
                {
                    name: 'topdomain.kz'
                },
                {
                    name: 'topdomain.kz'
                },
                {
                    name: 'topdomain.kz'
                },

                {
                    name: 'topdomain.kz'
                },

                {
                    name: 'topdomain.kz'
                },

                {
                    name: 'topdomain.kz'
                },

                {
                    name: 'topdomain.kz'
                },

                {
                    name: 'topdomain.kz'
                },

                {
                    name: 'topdomain.kz'
                },


            ]
        }
    },
    mounted() {
        for (let i = 0; i < 5; i++) {
            $(`#collapseExample${i}`).on("show.bs.collapse", function () {
                document.querySelector(`#arrow${i}`).classList.add = 'active'
            });
            $(`#collapseExample${i}`).on("hide.bs.collapse", function () {
                document.querySelector(`#arrow${i}`).classList.remove = 'active'
            });
            document.querySelector(`#arrow${i}`).onclick = () => {
                $(`#collapseExample${i}`).collapse("toggle");
            };


            this.parallax = new Parallax(this.$refs.scene);


        }
    },
    beforeUnmount() {
        // Удаляем Parallax.js перед размонтированием компонента
        this.parallax.destroy();
    },
}
</script >
<script setup>
useSeoMeta({
    title: 'Главная | Domain Market',
    ogTitle: 'Главная | Domain Market',
    description: 'Главная | Domain Market',
    ogDescription: 'Главная | Domain Market',
})
</script>

<style lang="scss" scoped>
.active {
    transform: rotate(90deg);
}

.faq {
    margin-top: 84px;

    h1 {
        font-size: 48px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        text-transform: uppercase;
        color: #fff;
        font-family: var(--cera);
        margin-bottom: 24px;
    }

    .faq__body {
        .faq__item {
            padding: 0 0 10px;
            border-bottom: 1px solid #fff;
            margin-bottom: 24px;

            button {
                width: 100%;
                text-align: left;
                background: transparent !important;
                border: 0;
                font-size: 18px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                font-family: var(--cera);
                color: #fff;
                outline: 0 !important;
                box-shadow: none !important;
                width: 53.385vw;
                display: flex;
                justify-content: space-between;
                align-items: center;
                padding: 0;
            }

            .card-body {
                max-width: 53.385vw;
                background: transparent;
                padding-left: 0;
                padding-bottom: 0;

                p {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 150%;
                    font-family: var(--cera);
                    color: #fff;

                }

                p:last-child {
                    margin-bottom: 0;
                }
            }
        }
    }
}

.reviews {
    position: relative;
    margin: 84px -150px 0;

    .swiper-pagination-fraction,
    .swiper-pagination-custom,
    .swiper-horizontal>.swiper-pagination-bullets,
    .swiper-pagination-bullets.swiper-pagination-horizontal {
        display: flex;
        justify-content: center;
        margin-top: 42px;
    }

    .prev {
        position: absolute;
        left: 15%;
        top: 30%;
        z-index: 10;
        transition: all .3s ease;
    }

    .next {
        position: absolute;
        right: 15%;
        top: 30%;
        z-index: 10;
        transition: all .3s ease;
    }

    .swiper-button-disabled {
        opacity: 0 !important;
    }

    .swiper-pagination-bullet {
        background: grey !important;
    }

    h1 {
        font-feature-settings: 'clig' off, 'liga' off;
        font-family: var(--cera);
        font-size: 48px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        color: #fff;
        margin-bottom: 48px;
        padding: 0 0 0 150px;
    }

    .slide {
        border-radius: 20px;
        background: #FFF;
        padding: 40px;
        width: 100%;
        height: 311px;
    }

    .name {
        span {
            font-size: 24px;
            font-style: normal;
            font-weight: 500;
            line-height: 120%;
            font-family: var(--cera);
            color: #333;
        }

        small {
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 120%;
            font-family: var(--cera);
            color: #333;
            opacity: 0.3;
        }
    }

    p {
        font-size: 18px;
        font-style: normal;
        font-weight: 400;
        line-height: 160%;
        color: #333;
        font-family: var(--cera);
        max-width: 663px;
        margin-top: 43px;
    }
}

.safety {
    margin-top: 84px;

    h1 {
        font-size: 48px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        text-transform: uppercase;
        font-family: var(--cera);
        color: #fff;
        margin-bottom: 36px;
    }

    div {
        display: flex;
        //align-items: flex-start;
        justify-content: space-between;


        div {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: flex-end;

            p {
                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 140%;
                font-family: var(--cera);
                color: #fff;
                margin-top: 103px;
                max-width: 851px;
            }

            div {
                text-align: right;

                a {
                    border-radius: 10px;
                    border: 2px solid #FFF;
                    padding: 10px 40px;
                    background: transparent;

                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    color: #fff;
                    font-family: var(--cera);
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
}

.catalog {
    display: flex;
    margin-top: 270px;
    gap: 30px 174px;

    h1 {
        font-size: 48px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        text-transform: uppercase;
        font-family: var(--cera);
        color: #fff;
        margin-bottom: 0;
    }

    .trap {
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        a {
            border-radius: 10px;
            border: 2px solid #FFF;
            background: transparent;
            padding: 11px 25px;
            text-align: center;
            text-decoration: none;

            font-size: 18px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            color: #Fff;
            font-family: var(--cera);
            transition: all .3s ease;

            &:hover {
                color: #000;
                background: #fff;
            }
        }
    }

    .catalog__body {
        display: flex;
        flex-wrap: wrap;
        gap: 36px;
        width: 100%;

        .catalog__item {
            padding: 8px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid #fff;
            width: 388px;
            text-decoration: none !important;
            transition: all .3s ease;

            p {
                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--cera);
                color: #fff;
            }

            img {
                transition: all .3s ease;
            }



            &:hover img {
                transform: scale(1.3);
            }
        }
    }
}

.home {
    padding: 150px 150px 72px;
}

.main {
    .parallax {
        position: relative;

        img {
            z-index: 10;
            position: relative;
        }

        span {
            position: absolute;
            color: #FFF;
            font-family: var(--cera);
            line-height: normal;

            &:nth-child(2) {
                font-size: 72px;
                font-weight: 500;
                left: -3.5% !important;
                top: 32% !important;
                z-index: 1;
            }

            &:nth-child(3) {
                font-size: 40px;
                font-weight: 300;
                left: 18% !important;
                top: 55% !important;
            }

            &:nth-child(4) {
                font-size: 48px;
                font-weight: 300;
                left: 17.3% !important;
                bottom: 18% !important;
                top: unset !important;
            }

            &:nth-child(5) {
                font-size: 72px;
                font-weight: 500;
                top: 45% !important;
                left: 45% !important;
                z-index: 11;
            }

            &:nth-child(6) {
                font-size: 64px;
                font-weight: 400;
                right: 37% !important;
                top: 13% !important;
                left: unset !important;
                z-index: 11;
            }

            &:nth-child(7) {
                font-size: 64px;
                font-weight: 400;
                right: 15% !important;
                top: 35% !important;
                left: unset !important;
                z-index: 11;
            }

            &:nth-child(8) {
                font-size: 64px;
                font-weight: 400;
                right: 5% !important;
                bottom: 23% !important;
                left: unset !important;
                top: unset !important;
                z-index: 11;
            }
        }


    }

    div {
        h2 {
            font-size: 72px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            color: #fff;
            font-family: var(--cera);
        }

        h1 {
            font-size: 72px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            font-family: var(--cera);
            color: #fff;
            display: inline;
            position: relative;

            span {
                background: linear-gradient(180deg, #D675DB 0%, #0051A3 100%);
                color: #000 !important;
                background-clip: text;
                -webkit-background-clip: text;
                -webkit-text-fill-color: transparent;
                z-index: 10;
            }

            img {
                position: absolute;
                right: 0;
                bottom: -10%;
                z-index: 0;
            }
        }

    }

    .search {
        margin-top: 100px;

        p {
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--cera);
            color: #fff;
            max-width: 578px;
        }

        input {
            border-radius: 10px;
            border: 2px solid #FFF;
            border-left: 0;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
            background: transparent;
            padding: 10px 12px;
            box-shadow: none;

            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            color: #fff;
            font-family: var(--cera);
            height: 44px;
        }

        .input-group-text {

            border-radius: 10px;
            border-top-right-radius: 0;
            border-bottom-right-radius: 0;
            border: 2px solid #FFF;
            border-right: 0;
            background: transparent;
            padding: 0;
            height: 44px;
        }

        button {
            font-size: 18px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            color: #fff;
            font-family: var(--cera);

            padding: 10px 60px 14px;
            max-height: 44px;

            border-radius: 10px;
            border: 2px solid #FFF;
            background: transparent;
            margin-left: 10px;
            transition: all .3s ease;
        }

        button:hover {
            color: #000;
            background: #fff;
        }
    }
}
</style>