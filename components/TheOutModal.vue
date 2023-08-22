<template>
    <div class="modal fade" id="outModal" tabindex="-1" aria-labelledby="outModal" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modalbody">
                    <div class="text-center opa">
                        <div></div>
                        <h1>Вывод средств</h1>
                        <img src="@/assets/img/closemodal.svg" alt="" style="cursor: pointer;" data-dismiss="modal"
                            aria-label="Close">
                    </div>
                    <div class="tabers" v-if="accountType == 'buyer'">
                        <button data-toggle="modal" data-dismiss="modal" aria-label="Close"
                            data-target="#inModal">ПОПОЛНЕНИЕ</button>
                        <button>ВЫВОД</button>
                    </div>
                    <p>Порядок действий для вывода средств</p>
                    <p>1. Подтвердите Ваше согласие с правилами нашей системы</p>

                    <label class="custom-checkbox">
                        <input type="checkbox" class="" value="0">
                        <p class="checkbox-text m-0">Принимаю условия <NuxtLink to='/terms'>Пользовательского соглашения
                            </NuxtLink> и<br>
                            <NuxtLink to='/polytics'>Политики
                                конфиденциальности</NuxtLink>
                        </p>
                    </label>
                    <p>2. Введите номер карты, на которую вы хотите вывести средства.</p>
                    <input type="text" class="mb-3 w-100" name="card" id="card" v-model="cardNumber"
                        :maxlength="cardNumberMaxLength" placeholder="Введите номер карты" @input="formatCardNumber"
                        autocomplete="cc-number">
                    <input type="text" class="mb-3 w-100" name="cardHolder" id="card" v-model="cardHolder"
                        placeholder="Введите владельца карты" autocomplete="cc-name">
                    <p>3. Введите сумму, которую Вы хотите вывести с личного счета, и нажмите на кнопку “Вывести”. Вы будете
                        переадресованы на сайт платежной системы, где сможете завершить операцию.</p>


                    <div class="clap">
                        <input type="number" placeholder="100 ₸" v-model="count">
                        <button ref="outBtn" @click="outMoney()">ВЫВЕСТИ</button>
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
import global from '~/mixins/global';
import axios from 'axios'
export default {
    mixins: [global],
    data() {
        return {
            cardNumber: '',
            cardNumberMaxLength: 19,
            count: null,
            cardHolder: '',
            pathUrl: 'https://d-market.kz',
            accountType: '',
        }
    },
    methods: {
        outMoney() {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/money/pay-return`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.outBtn.innerHTML = 'ОЖИДАЙТЕ'

            axios
                .post(path, {
                    amount: this.count,
                    card_number: this.cardNumber.replace(/\s/g, ''),
                    cardholder: this.cardHolder
                })
                .then(response => {
                    console.log(response)
                    if (response.status == 200) {
                        this.$refs.outBtn.innerHTML = 'УСПЕШНО'
                    }
                    if (response.status == 228) {
                        this.$refs.outBtn.innerHTML = response.data.error_msg
                    }

                })
                .catch(error => {
                    console.error(error)
                    this.$refs.outBtn.innerHTML = 'ВЫВЕСТИ'
                })
        },
        formatCardNumber() {
            // Удаляем все символы, кроме цифр
            this.cardNumber = this.cardNumber.replace(/\D/g, '');

            // Добавляем разделитель каждые 4 символа
            this.cardNumber = this.cardNumber.replace(/(.{4})/g, '$1 ');

            // Обрезаем карточный номер до максимальной длины
            this.cardNumber = this.cardNumber.slice(0, this.cardNumberMaxLength);
        }
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            this.accountType = 'buyer'

        }
        else if (accType == 'seller-account') {
            this.accountType = 'seller'
        }
        else {
            return
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
.tabers {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;

}

.tabers button {
    flex: 1;
    background: transparent;
    border: 3px solid #fff;
    border-radius: 10px;
    padding: 12px 0;
    text-align: center;

    font-size: 18px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    font-family: var(--cera);
}

.tabers button:nth-child(1) {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
    border-right: 0;

    color: #fff;
    background: transparent;
}

.tabers button:nth-child(2) {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    border-left: 0;
    color: #000;
    background: #fff;
}

.opa {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
}

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

@media (max-width: 1024px) {
    .modalbody {
        padding: 30px 20px;
    }

    .modalbody h1 {
        font-size: 24px;
    }

    .modalbody p {
        font-size: 16px;
    }

    .tabs {
        justify-content: center;
    }
}
</style>