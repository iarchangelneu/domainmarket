<template>
    <div class="create">
        <div class="create__body">
            <div class="left__side">
                <div class="input__body">
                    <label for="name">Название домена</label>
                    <input type="text" id="name" placeholder="Введите название домена" v-model="name">
                </div>
                <div class="d-flex align-items-center category__select">
                    <div>
                        <label for="category">Выберите категорию</label>
                        <select name="add_category" id="category" v-model="selectedCategory" :disabled="hasSelectedCategory"
                            required>
                            <option value="" disabled>Выбор категории</option>
                            <option v-for="(category, index) in categories" :value="index" :key="index">{{ category
                            }}
                            </option>
                        </select>
                    </div>

                    <div class="selected__cat" v-if="selectedCategories.length > 0">
                        <span class="d-flex align-items-start justify-content-between">
                            {{ categories[selectedCategories[0]] }}
                            <img src="@/assets/img/del.svg" @click="removeCategory"
                                style="cursor: pointer; margin-left: 10px;">

                        </span>
                    </div>
                </div>

                <div class="input__body te d-flex">
                    <div>
                        <label for="price">Введите цену</label>
                        <input type="number" id="price" placeholder="Цена за товар" v-model="price">
                    </div>
                    <div>
                        <label for="reg">Регистратор домена</label>
                        <input type="text" id="reg" placeholder="Введите название регистратора" v-model="registrator">
                    </div>
                </div>
            </div>

            <div class="right__side">
                <label for="desc">Описание товара</label>
                <textarea name="desc" id="desc" v-model="description" cols="30" rows="10"></textarea>
            </div>
        </div>

        <div class="text-center">
            <button>ОПУБЛИКОВАТЬ ТОВАР</button>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            name: '',
            categories: [
                "Развлечения",
                "Еда и рестораны",
                "Образование",
                "Одежда и мода",
                "Автомобили",
                "Путешествия",
                "Питомцы",
                "Предприятия",
                "Недвижимость",
                "Медицина",
                "Финансы",
                "Техника",
            ],
            selectedCategory: null,
            selectedCategories: [],
            description: '',
        }
    },
    methods: {
        removeCategory() {
            this.selectedCategories.splice(0, 1);
            this.selectedCategory = null;
        },
    },
    computed: {
        hasSelectedCategory() {
            return this.selectedCategories.length > 0;
        },
    },
    watch: {
        selectedCategory(newVal) {
            if (newVal !== null) {
                this.selectedCategories = [newVal];
            }
        },
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Редактирование товара | Domain Market',
    ogTitle: 'Редактирование товара | Domain Market',
    description: 'Редактирование товара | Domain Market',
    ogDescription: 'Редактирование товара | Domain Market',
})
</script>
<style lang="scss" scoped>
.create {
    margin-top: 36px;

    button {
        padding: 12px 105px;
        background: transparent;
        border-radius: 10px;
        border: 2px solid #FFF;

        font-size: 18px;
        font-style: normal;
        font-weight: 400;
        line-height: 110%;
        font-family: var(--cera);
        color: #fff;
        margin-top: 36px;
        transition: all .3s ease;

        &:hover {
            color: #000;
            background: #fff;
        }
    }

    .create__body {
        display: flex;
        gap: 40px;



        label {
            display: block;
            font-size: 18px;
            font-style: normal;
            font-weight: 400;
            line-height: 110%;
            color: #fff;
            font-family: var(--cera);
            margin-bottom: 10px;
            margin-left: 20px;
        }

        .right__side {
            width: 100%;

            textarea {
                width: 100%;
                border-radius: 10px;
                border: 2px solid #FFF;
                background: transparent;
                padding: 20px;

                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--cera);
                color: #fff;
            }
        }

        .left__side {
            width: 100%;

            .te {

                width: 100%;
                gap: 20px;

                #price {
                    width: 14.167vw;
                }

                #reg {
                    width: 23.906vw;
                }

            }

            input,
            select {
                display: block;
                border-radius: 10px;
                border: 2px solid #FFF;
                background: transparent;
                padding: 10px 20px;
                width: 100%;

                color: #fff;
                font-size: 18px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--cera);
                margin-bottom: 20px;
            }

            input {
                width: 39.115vw;
            }

            select {
                color: #fff;
                width: 25.885vw;
            }

            option {
                color: #000;
            }

            .category__select {
                width: 100%;
            }

            .selected__cat {
                border-radius: 10px;
                border: 2px solid #fff;
                padding: 13px 20px;
                margin-top: 9px;
                margin-left: 20px;
                width: 12.188vw;


                span {
                    color: #fff;
                    font-size: 18px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 110%;
                    font-family: var(--cera);
                }
            }
        }
    }
}
</style>