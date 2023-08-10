<template>
    <div class="modal fade" id="inModal" tabindex="-1" aria-labelledby="inModal" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modalbody">
                    <div class="text-center">
                        <h1>Пополнение баланса</h1>
                    </div>

                    <p>Порядок действий для пополнения счета</p>
                    <p>1. Подтвердите Ваше согласие с правилами нашей системы</p>

                    <label class="custom-checkbox">
                        <input type="checkbox" class="" value="0">
                        <p class="checkbox-text m-0">Принимаю условия <NuxtLink to='/terms'>Пользовательского соглашения
                            </NuxtLink> и<br>
                            <NuxtLink to='/polytics'>Политики
                                конфиденциальности</NuxtLink>
                        </p>
                    </label>
                    <p>2. Введите сумму, на которую Вы хотите пополнить личный счет, и нажмите на кнопку “Пополнить”. Вы
                        будете переадресованы на сайт платежной системы, где сможете завершить платеж.</p>


                    <div class="clap">
                        <input type="number" placeholder="100 ₸" v-model="count">
                        <button>ПОПОЛНИТЬ</button>
                    </div>

                    <div class="tabs">
                        <div class="tab" :class="{ active: count == 100 }" @click="count = 100">100 ₸</div>
                        <div class="tab" :class="{ active: count == 1000 }" @click="count = 1000">1000 ₸</div>
                        <div class="tab" :class="{ active: count == 2000 }" @click="count = 2000">2000 ₸</div>
                        <div class="tab" :class="{ active: count == 5000 }" @click="count = 5000">5000 ₸</div>
                        <div class="tab" :class="{ active: count == 10000 }" @click="count = 10000">10000 ₸</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            cardNumber: '',
            cardNumberMaxLength: 19,
            count: null,
        }
    },
    methods: {
        formatCardNumber() {
            // Удаляем все символы, кроме цифр
            this.cardNumber = this.cardNumber.replace(/\D/g, '');

            // Добавляем разделитель каждые 4 символа
            this.cardNumber = this.cardNumber.replace(/(.{4})/g, '$1 ');

            // Обрезаем карточный номер до максимальной длины
            this.cardNumber = this.cardNumber.slice(0, this.cardNumberMaxLength);
        }
    }
}
</script >
<script setup>
useSeoMeta({
    title: 'Вывод средств | Domain Market',
    ogTitle: 'Вывод средств | Domain Market',
    description: 'Вывод средств | Domain Market',
    ogDescription: 'Вывод средств | Domain Market',
})
</script>
<style scoped>
.tabs {
    margin-top: 10px;
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
}

.tab {
    border-radius: 10px;
    border: 2px solid #FFF;
    background: transparent;
    cursor: pointer;
    padding: 10px 15px;

    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    color: #fff;
    font-family: var(--cera);
    transition: all .3s ease;
}

.tab:hover {
    color: #000;
    background: #fff;
}

.active {
    color: #000 !important;
    background: #fff !important;
}

.clap {
    display: flex;
    gap: 10px;
}

.clap input {
    text-align: right !important;
}

.clap button {
    padding: 11px 0;
    width: 100%;
    background: transparent;
    border-radius: 10px;
    border: 2px solid #FFF;

    font-size: 18px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--cera);
    color: #fff;
    transition: all .3s ease;

}

.clap button:hover {
    color: #000;
    background: #fff;
}

input {
    width: 100%;
    border-radius: 10px;
    border: 2px solid #FFF;
    background: transparent;
    padding: 8px 17px;

    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    color: #fff;
    font-family: var(--cera);
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
    font-size: 18px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--cera);
    color: #fff;
}

.custom-checkbox {
    margin-bottom: 30px;
}

.modalbody {
    padding: 38px;
}

.modalbody h1 {
    font-size: 24px;
    font-style: normal;
    font-weight: 500;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--cera);
    color: #fff;
    margin-bottom: 24px;
}

.modalbody p {
    font-size: 18px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--cera);
    color: #fff;
    margin-bottom: 24px;
}

.modal-content {
    border-radius: 30px;
    background: linear-gradient(285deg, #070707 0%, #262626 100%);
    border: 0;
}

.modal-dialog {
    max-width: 611px;
    margin: 1.75rem auto;
}
</style>