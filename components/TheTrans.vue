<template>
    <div class="transactions">
        <div class="trans__empty" v-if="test">
            <h1>СПИСОК ТРАНЗАКЦИЙ ОТОБРАЗИТСЯ ПОСЛЕ ПЕРВОЙ ПРОДАЖИ</h1>
        </div>
        <table class="table table-borderless text-center" v-else>
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
                <tr>
                    <td>Продажа</td>
                    <td>+ 8 000 ₸</td>
                    <td>05.12.22 15:47</td>
                    <td class="success">Совершено</td>
                    <td>18 000 ₸</td>
                </tr>
                <tr>
                    <td>Продажа</td>
                    <td>+ 8 000 ₸</td>
                    <td>05.12.22 15:47</td>
                    <td class="failure">Отменено</td>
                    <td>18 000 ₸</td>
                </tr>
                <tr>
                    <td>Продажа</td>
                    <td>+ 8 000 ₸</td>
                    <td>05.12.22 15:47</td>
                    <td class="success">Совершено</td>
                    <td>18 000 ₸</td>
                </tr>
                <tr>
                    <td>Продажа</td>
                    <td>+ 8 000 ₸</td>
                    <td>05.12.22 15:47</td>
                    <td class="failure">Отменено</td>
                    <td>18 000 ₸</td>
                </tr>

            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    data() {
        return {
            test: false,
        }
    },
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