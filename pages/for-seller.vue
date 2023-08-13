<template>
    <div class="seller">
        <div class="seller__body">
            <img src="@/assets/img/seller.png" alt="">
            <div>
                <h1>Продавай домены легко!</h1>
                <p>Domain Market - это современная платформа, которая дает возможность просто и быстро продавать доменные
                    имена</p>
                <ol>
                    <li>Зарегистрируйся как продавец на Domain Market</li>
                    <li>Перейдите в раздел “Мои товары” в Личном кабинете</li>
                    <li> Нажмите “Добавить товар” и заполните все специальные поля</li>
                    <li>В описании подробно расскажите для каких сайтов может использоваться этот домен</li>
                    <li>Укажите категорию (тематику) домена, чтобы пользователям проще было найти домен в общем каталоге
                    </li>
                    <li>Введите цену на домен</li>
                    <li>кажите регистратор домена</li>
                    <li>Опубликуй товар и ожидай продажи твоих доменов!</li>
                </ol>

                <p class="mt-4">Успешная продажа домена требует тщательной подготовки, профессионального общения и четкой
                    организации.
                    Следуй этой краткой инструкции и будь готов к активной коммуникации с потенциальными покупателями!</p>
                <div class="tre">
                    <NuxtLink to="/register">ЗАРЕГИСТРИРОВАТЬСЯ</NuxtLink>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
useSeoMeta({
    title: 'Продавцу | Domain Market',
    ogTitle: 'Продавцу | Domain Market',
    description: 'Продавцу | Domain Market',
    ogDescription: 'Продавцу | Domain Market',
})
</script>
<style lang="scss" scoped>
.seller {
    padding: 120px 150px 41px;

    @media (max-width: 1600px) {
        padding: 120px 50px 41px;
    }

    @media (max-width: 1024px) {
        padding: 120px 20px 40px;
    }

    .seller__body {
        display: flex;
        gap: 44px;

        @media (max-width: 1440px) {
            img {
                max-width: 30%;
                height: auto;
            }
        }

        @media (max-width: 1024px) {
            img {
                display: none;
            }
        }

        div {
            h1 {
                font-size: 48px;
                font-style: normal;
                font-weight: 500;
                line-height: 110%;
                font-family: var(--cera);
                color: #fff;
                margin-bottom: 36px;

                @media (max-width: 1024px) {
                    font-size: 24px;
                }
            }

            p {
                margin-bottom: 36px !important;

                @media (max-width: 1024px) {
                    margin-bottom: 15px !important;
                }
            }


            p,
            ul,
            li,
            ol {
                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                color: #fff;
                font-family: var(--cera);
                margin-bottom: 10px;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }

            a {
                text-decoration: none;
                padding: 12px 146px;
                border-radius: 10px;
                border: 2px solid #FFF;
                background: transparent;
                display: inline-block;

                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 110%;
                font-family: var(--cera);
                color: #fff;
                transition: all .3s ease;
                margin-top: 36px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                    padding: 10px 23px;
                    margin-top: 30px;
                }

                &:hover {
                    color: #000;
                    background: #fff;
                }
            }

            .tre {
                @media (max-width: 1024px) {
                    text-align: center;
                }
            }
        }
    }
}
</style>