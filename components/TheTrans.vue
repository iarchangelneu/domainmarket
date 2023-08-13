<template>
    <div class="transactions">
        <div class="trans__empty" v-if="transactions.length <= 0">
            <h1>СПИСОК ТРАНЗАКЦИЙ ОТОБРАЗИТСЯ ПОСЛЕ ПЕРВОЙ ПРОДАЖИ</h1>
        </div>
        <table class="pctrans table table-borderless text-center" v-else>
            <thead>
                <tr>
                    <th scope="col">Тип</th>
                    <th scope="col">Цена</th>
                    <th scope="col">Дата и Время</th>
                    <th scope="col">Статус</th>
                    <th scope="col">Состояние счета</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in transactions" :key="item.id">
                    <td>{{ item.type_operation }}</td>
                    <td> {{ item.amount.toLocaleString() }} ₸</td>
                    <td>{{ formatDate(item.date) }}</td>
                    <td :class="{ success: item.paid == 1, failure: item.paid == 0 }">{{ item.paid == 1 ? 'Совершено' :
                        item.paid == 0 ? 'Отменено' : item.paid }}</td>
                    <td>{{ calculateAmountNow(item) }} ₸</td>
                </tr>
            </tbody>
        </table>

        <div class="mobtrans">
            <div class="trans__item" v-for="item in transactions" :key="item.id">
                <div class="gle">
                    <div class="name">
                        <span>{{ item.type_operation }}</span>

                        <div class="isda">
                            <small>{{ formatDate(item.date) }}</small>
                            <img src="@/assets/img/succes.svg" alt="" v-if="item.paid == 1">
                            <img src="@/assets/img/failure.svg" alt="" v-if="item.paid == 0">
                        </div>
                    </div>
                    <div class="price">
                        <div>
                            <span>Цена</span>
                            <small>{{ item.amount.toLocaleString() }} ₸</small>
                        </div>
                        <div>
                            <span>Состояние счета</span>
                            <small>{{ calculateAmountNow(item) }} ₸</small>
                        </div>
                    </div>
                </div>
                <hr>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        transactions: [],
    },
    data() {
        return {
            test: false,
        }
    },
    methods: {
        calculateAmountNow(item) {
            if (item.type_operation === 'ВЫВОД' && item.paid === 0) {
                return (item.amount_now + item.amount).toLocaleString();
            }
            else if (item.type_operation === 'ПОПОЛНЕНИЕ' && item.paid === 0) {
                return (item.amount_now - item.amount).toLocaleString();
            }
            else {
                return item.amount_now.toLocaleString();
            }
        },
        formatDate(dateTime) {
            // Создаем объект Date из строки времени
            const date = new Date(dateTime);

            // Получаем отдельные компоненты даты и времени
            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = String(date.getFullYear()).slice(-2);
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');

            // Собираем строку в нужном формате
            return `${day}.${month}.${year} ${hours}:${minutes}`;
        },
    }
}
</script >
<script setup>
useSeoMeta({
    title: 'Ваши транзакции | Domain Market',
    ogTitle: 'Ваши транзакции | Domain Market',
    description: 'Ваши транзакции | Domain Market',
    ogDescription: 'Ваши транзакции | Domain Market',
})
</script>
<style lang="scss" scoped>
.transactions {
    margin-top: 36px;

    .pctrans {
        @media (max-width:1024px) {
            display: none;
        }
    }

    .mobtrans {
        display: none;

        @media (max-width: 1024px) {
            display: block;
            border-radius: 30px;
            border: 2px solid #FFF;
        }

        .trans__item {
            overflow: hidden;

            hr {
                border: 1px solid #333;
                margin: 0 1.5px 1rem;
            }

            .gle {
                padding: 30px 30px 20px;

                .name {
                    display: flex;
                    justify-content: space-between;

                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--cera);
                        color: #fff;
                        opacity: 0.5;
                    }

                    .isda {
                        display: flex;
                        gap: 20px;
                        justify-content: center;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--cera);
                            color: #fff;
                        }
                    }
                }

                .price {
                    display: flex;
                    align-items: center;
                    justify-content: space-between;
                    margin-top: 15px;

                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        color: #fff;
                        opacity: 0.5;
                        font-family: var(--cera);
                        display: block;

                    }

                    small {
                        font-family: var(--cera);
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        color: #fff;
                        font-family: var(--cera);
                        margin-top: 10px;
                        display: block;
                    }


                }

            }
        }
    }

    .trans__empty {
        display: flex;
        justify-content: center;
        margin-top: 100px;

        h1 {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 150%;
            color: #fff;
            font-family: var(--cera);
            text-align: center;
        }
    }

    th {
        font-size: 20px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        font-family: var(--cera);
        color: #fff;
    }

    td {
        font-size: 18px;
        font-style: normal;
        font-weight: 400;
        line-height: normal;
        text-transform: uppercase;
        color: #fff;
        font-family: var(--cera);
    }

    .success {
        color: #03B820;
    }

    .failure {
        color: #EA0505;
    }
}
</style>