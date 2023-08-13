<template>
    <div class="catalog">
        <h1>Каталог</h1>

        <div class="catalog__filters">
            <button @click="openSort = !openSort">
                <span>СОРТИРОВКА</span>
                <img src="@/assets/img/filtersarrow.svg" alt="">
            </button>
            <button @click="openFilter = !openFilter">
                <span>ФИЛЬТРЫ</span>
                <img src="@/assets/img/filtersarrow.svg" alt="">
            </button>
            <div class="sortbody text-center" :class="{ active: openSort }">
                <h2 @click="sortBy('price')">СНАЧАЛА ДЕШЕВЛЕ</h2>
                <img src="@/assets/img/hr.svg" alt="">
                <h2 @click="sortBy('-price')">СНАЧАЛА ДОРОЖЕ</h2>
                <img src="@/assets/img/hr.svg" alt="">
                <h2 @click="sortBy('-date_activate')">САМЫЕ НОВЫЕ</h2>
                <img src="@/assets/img/hr.svg" alt="">
                <h2 @click="sortBy('date_activate')">САМЫЕ СТАРЫЕ</h2>
            </div>
            <div class="filterbody" :class="{ active: openFilter }">
                <div class="price">
                    <h3>ЦЕНА</h3>

                    <div class="inputs">
                        <div class="inputs">
                            <input type="number" placeholder="От" v-model="minPrice" @input="applyFilters">
                            <img src="@/assets/img/lines.svg" alt="">
                            <input type="number" placeholder="До" v-model="maxPrice" @input="applyFilters">
                        </div>
                    </div>
                </div>

                <div class="cats">
                    <div class="domains">
                        <h3>ДОМЕННЫЕ ЗОНЫ</h3>
                        <div class="domains__block">
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                            <div class="domains__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" class="" value="0">
                                    <p class="checkbox-text m-0">.kz</p>
                                </label>
                            </div>
                        </div>
                    </div>
                    <div class="category">
                        <h3>КАТЕГОРИИ</h3>
                        <div class="category__block">
                            <div v-for="(category, index) in categories" :key="index" class="category__item">
                                <label class="custom-checkbox">
                                    <input type="checkbox" :value="category.id" v-model="selectedCategories"
                                        @change="applyFilters">
                                    <p class="checkbox-text m-0">{{ category.name }}</p>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div>
            <div v-if="products.length <= 0"></div>
            <div class="catalog__body" v-else>
                <div class="catalog__item" v-for="(product, i) in products.results" :key="product.id">
                    <div class="name">
                        <div>
                            <small>{{ i + 1 }}.</small>
                            <p>{{ product.name }}</p>
                        </div>
                        <span>{{ product.price.toLocaleString() }} ₸</span>
                    </div>
                    <div class="links">
                        <button @click="addToCart(product.id)" ref="cartBtn">В КОРЗИНУ</button>
                        <NuxtLink :to="'/product/' + product.id">СТРАНИЦА ТОВАРА</NuxtLink>
                    </div>
                </div>
            </div>

            <div class="text-center showmore">
                <button @click="loadMoreProducts()" ref="showmore">ПОКАЗАТЬ ЕЩЕ</button>
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
            products: [

            ],
            pathUrl: 'https://d-market.kz',
            openFilter: false,
            openSort: false,
            minPrice: null,
            maxPrice: null,
            selectedCategories: [],
            categories: [
                { id: 1, name: 'РАЗВЛЕЧЕНИЯ' },
                { id: 2, name: 'ЕДА И РЕСТОРАНЫ' },
                { id: 3, name: 'ОБРАЗОВАНИЕ' },
                { id: 4, name: 'ОДЕЖДА И МОДА' },
                { id: 5, name: 'АВТОМОБИЛИ' },
                { id: 6, name: 'ПУТЕШЕСТВИЯ' },
                { id: 7, name: 'ПИТОМЦЫ' },
                { id: 8, name: 'ПРЕДПРИЯТИЯ' },
                { id: 9, name: 'НЕДВИЖИМОСТЬ' },
                { id: 10, name: 'МЕДИЦИНА' },
                { id: 11, name: 'ФИНАНСЫ' },
                { id: 12, name: 'ТЕХНИКА' },
            ],
        }
    },
    methods: {
        addToCart(id) {
            const path = `${this.pathUrl}/api/buyer/add-product-basket`
            this.$refs.cartBtn.innerHTML = 'Добавляем'
            axios
                .post(path, {
                    products: id,
                    amount: 1,
                })
                .then(response => {
                    if (response.status == 201) {
                        this.$refs.cartBtn.disabled = true
                        this.$refs.cartBtn.innerHTML = 'Добавлен'
                        this.getCart()
                    }
                    else {
                        this.$refs.cartBtn.innerHTML = 'Неудача'
                        this.$refs.cartBtn.disabled = false

                    }
                })
                .catch(error => {
                    console.error(error)
                })
        },
        loadMoreProducts() {
            if (this.products.next) {
                axios
                    .get(this.products.next)
                    .then(response => {
                        // Добавляем новые продукты к существующим
                        this.products.results.push(...response.data.results);
                        this.products.next = response.data.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        async applyFilters() {
            let queryParams = '';

            if (this.minPrice !== null && this.maxPrice !== null) {
                queryParams += `price__gte=${this.minPrice}&price__lte=${this.maxPrice}`;
            }

            if (this.selectedCategories.length > 0) {
                const categoriesQueryParam = this.selectedCategories.join(',');
                queryParams += `&category__in=${categoriesQueryParam}`;
            }

            const response = await fetch(`${this.pathUrl}/api/products/all-product?${queryParams}`);
            const data = await response.json();
            this.products = data;
        },

        async sortBy(ordering) {
            const response = await fetch(`${this.pathUrl}/api/products/all-product?ordering=${ordering}`);
            const data = await response.json();
            this.products = data; // Обновляем массив products с отсортированными данными
        },
        getProducts() {
            const token = this.getAuthorizationCookie();
            const path = `${this.pathUrl}/api/products/all-product`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            if (this.$route.query.search) {
                axios
                    .get(`${path}?name__icontains=${this.$route.query.search}`)
                    .then(response => {
                        this.products = response.data;
                    })
                    .catch(error => {
                        console.log(error);
                    });
            } else {
                axios
                    .get(path)
                    .then(response => {
                        this.products = response.data;
                    })
                    .catch(error => {
                        console.log(error);
                    });
            }
        },
    },
    mounted() {
        this.getProducts()
    },
}
</script>
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

    @media (max-width: 1500px) {
        padding: 120px 50px 72px;
    }

    @media (max-width: 1024px) {
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

        @media (max-width: 1500px) {
            justify-content: center;
        }

        @media (max-width: 1024px) {
            margin-top: 20px;
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



    .catalog__filters {
        display: flex;
        justify-content: flex-end;
        gap: 20px;
        position: relative;

        @media (max-width: 1024px) {
            justify-content: center;
            margin-top: 20px;
        }

        .active {
            transform: scaleY(1) !important;
        }

        .sortbody {
            position: absolute;
            top: 150%;
            right: 11%;
            border-radius: 10px;
            transform: scaleY(0);
            border: 2px solid #FFF;
            background: #FFF;
            padding: 30px;
            transition: all .3s ease;

            h2 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--cera);
                color: #000;
                cursor: pointer;
                margin: 0;
            }

            img {
                margin: 10px 0;
            }

        }

        .filterbody {
            position: absolute;
            border-radius: 10px;
            border: 2px solid #FFF;
            background: #FFF;
            padding: 30px;
            top: 160%;
            right: 0;
            transform: scaleY(0);
            transition: all .3s ease;

            @media (max-width: 1024px) {
                padding: 20px;
            }

            .cats {
                display: flex;
                gap: 108px;
                margin-top: 20px;

                @media (max-width: 1024px) {
                    gap: 30px;
                    flex-direction: column;
                }

                h3 {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--cera);
                    color: #000;
                    margin-bottom: 10px;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                p {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 110%;
                    text-transform: uppercase;
                    color: #000;
                    font-family: var(--cera);

                    @media (max-width: 1024px) {
                        font-size: 14px;
                        white-space: nowrap;
                    }
                }

                .domains__block {
                    display: grid;
                    grid-template-columns: 1fr 1fr;
                    grid-template-areas:
                        ". ."
                        ". ."
                        ". .";
                    gap: 10px 30px;

                }

                .category__block {
                    display: grid;
                    grid-template-columns: 1fr 1fr;
                    grid-template-areas:
                        ". ."
                        ". ."
                        ". .";
                    gap: 10px 30px;

                    @media (max-width: 1024px) {
                        gap: 10px;
                    }
                }
            }

            .price {
                h3 {
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--cera);
                    color: #000;
                    margin-bottom: 10px;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                .inputs {
                    display: flex;
                    align-items: center;
                    gap: 10px;

                    input {
                        border-radius: 10px;
                        border: 2px solid #000;
                        background: transparent;
                        padding: 10px;
                        text-align: right;

                        font-size: 24px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 110%;
                        color: #000;
                        font-family: var(--cera);
                        max-width: 140px;

                        @media (max-width: 1024px) {
                            font-size: 18px;
                        }
                    }
                }
            }
        }

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

            @media (max-width: 1024px) {
                font-size: 16px;
                padding: 10px 17px;
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
    content: '';
    display: inline-block;
    vertical-align: middle;
    width: 20px;
    height: 20px;
    margin-right: 12px;
    border: 2px solid #000;
    border-radius: 4px;
    margin-bottom: 4px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
    content: url('@/assets/img/checkwhite.svg');
    font-size: 16px;
    color: black;
    text-align: center;
    line-height: 20px;
    background: #000;
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
    color: #000;
}
</style>