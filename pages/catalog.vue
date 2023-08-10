<template>
    <div class="catalog">
        <h1>Каталог</h1>

        <div class="catalog__filters">
            <button>
                <span>СОРТИРОВКА</span>
                <img src="@/assets/img/filtersarrow.svg" alt="">
            </button>
            <button>
                <span>ФИЛЬТРЫ</span>
                <img src="@/assets/img/filtersarrow.svg" alt="">
            </button>
        </div>

        <div>
            <div class="catalog__body">
                <div class="catalog__item" v-for="(product, i) in products" :key="product.i">
                    <div class="name">
                        <div>
                            <small>{{ i + 1 }}.</small>
                            <p>{{ product.name }}</p>
                        </div>
                        <span>{{ product.price.toLocaleString() }} ₸</span>
                    </div>
                    <div class="links">
                        <button>В КОРЗИНУ</button>
                        <NuxtLink to="/product">СТРАНИЦА ТОВАРА</NuxtLink>
                    </div>
                </div>
            </div>

            <div class="text-center showmore">
                <button>ПОКАЗАТЬ ЕЩЕ</button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
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
    }
}
</script >
<script setup>
useSeoMeta({
    title: 'Каталог | Domain Market',
    ogTitle: 'Каталог | Domain Market',
    description: 'Каталог | Domain Market',
    ogDescription: 'Каталог | Domain Market',
})
</script>
<style lang="scss" scoped>
.catalog {
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

    .showmore {
        margin-top: 36px;

        button {
            border-radius: 10px;
            border: 2px solid #FFF;
            padding: 12px 60px;
            background: transparent;

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

    .catalog__body {
        display: flex;
        flex-wrap: wrap;
        gap: 36px 95px;
        margin-top: 40px;

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

    .catalog__filters {
        display: flex;
        justify-content: flex-end;
        gap: 20px;

        button {
            display: flex;
            align-items: center;
            gap: 10px;

            border-radius: 10px;
            border: 2px solid #FFF;
            padding: 12px 24px;
            background: transparent;

            color: #fff;
            font-family: var(--cera);
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
        }
    }
}
</style>