<template>
    <section class="projects">
        <div class="container">
            <h2 class="title">Кейсы</h2>
            <ul class="list-reset projects__items">
                <li class="projects__item" v-for="category in categories" :key="category.id">
                    <a class="projects__link" href="#" @click.prevent="selectCategory(category.id)">
                        {{ category.name }}
                    </a>
                </li>
            </ul>
        </div>
    </section>
</template>S

<script setup>
import { ref, onMounted} from 'vue';

const categories = ref([]);
const selectedCategory = ref(null);

const emit = defineEmits(['update:selectCategory']) // определяем событие

const selectCategory = (categoryId) => {
    selectedCategory.value = categoryId;
    emit('update:selectCategory', categoryId);
    console.log('Selected Category:', selectedCategory.value);
}

const fetchCategories = async () => {
    try {
        const response = await fetch ('https://api.test.cyberia.studio/api/v1/project-categories');
        const data = await response.json();
        console.log('API response', data);
        categories.value = data.items;
        console.log('Categories:', categories.value)
    } catch (error) {
        console.log('Ошибка при выполнении fetch запроса:', error)
    }
}

onMounted(fetchCategories)
</script>

<style lang="scss">

    .title {
        margin: 0;
        margin-bottom: 72px;
        font-weight: 600;
        font-size: 43px;
        color: #eef3ff;
    }

    .projects {
        padding-top: 130px;
        

        &__items {
            margin: 0;
            margin-bottom: 52px;
            padding: 0;
            display: flex;
            justify-content: space-between;
        }

        &__item {
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 7px;
            background-color: #313341;
            cursor: pointer;
            box-shadow: 8px 15px 29px 0 rgba(23, 27, 44, 0.41);
            transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out, background 0.3s ease-in-out, border-radius 0.3s ease-in-out, outline 0.3s ease-in-out, outline-offset 0.3s ease-in-out;

            &:focus {
                border-radius: 7px;
                border-top-left-radius: 7px;
                border-bottom-left-radius: 7px;
                border-top-right-radius: 7px;
                border-bottom-right-radius: 7px;
            }

            &:hover {
                border-top-left-radius: 7px;
                border-bottom-left-radius: 7px;
                border-top-right-radius: 7px;
                border-bottom-right-radius: 7px;
                transform: scale(1.05);
                box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2);
                background: #2d76f9;
            }

            &:active {
                border-top-left-radius: 7px;
                border-bottom-left-radius: 7px;
                border-top-right-radius: 7px;
                border-bottom-right-radius: 7px;
                background: #2d76f9;
                border-radius: 7px;
            }

        }

        &__link {
            padding: 13px 29px;
            font-weight: 400;
            font-size: 21px;
            color: #eef3ff;
            letter-spacing: 1px;
            text-decoration: none;

            &:focus {
                border-top-left-radius: 7px;
                border-bottom-left-radius: 7px;
                border-top-right-radius: 7px;
                border-bottom-right-radius: 7px;
                background: #3475ee;
                display: flex;
                justify-content: center;
                align-items: center;
                width: 100%;
            }
        }



    }


    @media (max-width:1120px) {

        .projects {
            &__items {
                justify-content: center;
                align-items: center;
                display: flex;
                flex-wrap: wrap;
                gap: 30px;
            }

            &__item {
                width: calc((100% - (40px * 1)) / 2);
            }
        }
    }

    @media (max-width: 768px) {

        .projects {
            padding-top: 70px;
        }
    }

    @media (max-width: 375px) {

        .logo__img {
            max-width: 93px;
        }

        .projects {
            padding-top: 28px;
            padding-bottom: 40px;

            &__items {
                margin: 0;
                gap: 8px;
                row-gap: 18px;
            }

            &__item {
                width: calc((100% - 8px) / 2);
                box-shadow: 8px 15px 29px 0 rgba(23, 27, 44, 0.41);
            }

            &__link {
                padding: 10px 0px;
                font-size: 11px;
            }

        }

        .title {
            margin-bottom: 48px;
            font-weight: 500;
            font-size: 21px;
            letter-spacing: 0.7px;
        }
    }
</style>