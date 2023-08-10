<template>
    <div class="product">
        <div class="product__body">
            <div class="product__side">
                <div class="name">
                    <h1>EkoTourKazakhstan.com</h1>
                    <span>11 540 ₸</span>
                </div>

                <div class="product__description">
                    <h2>Описание:</h2>

                    <p>Продается привлекательный и многогранный домен EkoTourKazakhstan.com, идеальный выбор для разработки
                        веб-сайта, посвященного экологическому туризму и природным красотам Казахстана. Этот доменный адрес
                        идеально подойдет для турфирм, гидов, отелей, а также организаций и органов, связанных с развитием
                        экологического и устойчивого туризма.</p>

                    <p>Продается привлекательный и многогранный домен EkoTourKazakhstan.com, идеальный выбор для разработки
                        веб-сайта, посвященного экологическому туризму и природным красотам Казахстана. Этот доменный адрес
                        идеально подойдет для турфирм, гидов, отелей, а также организаций и органов, связанных с развитием
                        экологического и устойчивого туризма.</p>
                    <p>Продается привлекательный и многогранный домен EkoTourKazakhstan.com, идеальный выбор для разработки
                        веб-сайта, посвященного экологическому туризму и природным красотам Казахстана. Этот доменный адрес
                        идеально подойдет для турфирм, гидов, отелей, а также организаций и органов, связанных с развитием
                        экологического и устойчивого туризма.</p>
                    <p>Продается привлекательный и многогранный домен EkoTourKazakhstan.com, идеальный выбор для разработки
                        веб-сайта, посвященного экологическому туризму и природным красотам Казахстана. Этот доменный адрес
                        идеально подойдет для турфирм, гидов, отелей, а также организаций и органов, связанных с развитием
                        экологического и устойчивого туризма.</p>

                </div>

            </div>
            <div class="seller__side">
                <div class="seller__info">
                    <small>Автор:</small>
                    <span>Александр Иванов</span>
                </div>
                <div class="seller__info">
                    <small>Категория:</small>
                    <span>Путешествия</span>
                </div>
                <div class="seller__info">
                    <small>Регистратор домена:</small>
                    <span>ps.kz</span>
                </div>

                <button>В КОРЗИНУ</button>
            </div>
        </div>
    </div>
</template>
<script setup>
useSeoMeta({
    title: 'Товар | Domain Market',
    ogTitle: 'Товар | Domain Market',
    description: 'Товар | Domain Market',
    ogDescription: 'Товар | Domain Market',
})
</script>
<style lang="scss" scoped>
.product {
    padding: 120px 150px;

    .product__body {
        margin-top: 50px;
        display: flex;
        justify-content: space-between;
        gap: 60px;

        .seller__side {
            width: 100%;
            max-width: 529px;

            .seller__info {
                display: flex;
                justify-content: space-between;
                align-items: center;
                margin-bottom: 24px;

                small {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--cera);
                    color: #fff;
                }

                span {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    color: #fff;
                    font-family: var(--cera);
                    padding: 7px 20px;
                    border-radius: 10px;
                    border: 2px solid #FFF;
                }
            }

            button {
                width: 100%;
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
                    background: #fff;
                    color: #000;
                }
            }
        }

        .product__side {
            width: 100%;
            max-width: 1041px;

            .name {
                display: flex;
                justify-content: space-between;
                align-items: center;

                h1 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--cera);
                    color: #fff;
                    margin: 0;
                }

                span {
                    font-size: 36px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--cera);
                    color: #fff;
                }
            }

            .product__description {
                margin: 43px 0 0;

                h2 {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    color: #fff;
                    font-family: var(--cera);
                }

                p {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--cera);
                    color: #fff;
                    max-width: 1041px;
                    margin-top: 10px;
                }
            }
        }
    }
}
</style>