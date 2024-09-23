<template>
    <section class="feedback">
        <div class="container">
            <h2 class="title feedback__title">
                Расскажите о&nbsp;вашем проекте:
            </h2>
            <form @submit.prevent="submitForm"  action="" class="form">
                <div class="form__block">
                    <label class="form__label" for="name">
                        <span v-if="should.name" class="label__span span__name">Ваше имя*</span>
                        <input v-model="formDate.name" @focus="hideLabel('name')" @input="hideLabel('name'), clearError('name')" @blur="showLabel('name')" class="form__input" id="name" type="text" >
                        <span v-if="errors.name" class="error">{{ errors.name }}</span>
                    </label>

                    <label class="form__label" for="email">
                        <span v-if="should.email" class="label__span span__email">Email*</span>
                        <input v-model="formDate.email" @focus="hideLabel('email')" @input="hideLabel('email'), clearError('email')" @blur="showLabel('email')" class="form__input" id="email" type="email" >
                        <span v-if="errors.email" class="error">{{ errors.email }}</span>
                    </label>

                    
                    <label class="form__label" for="tel">
                        <div>
                            <span v-if="should.phone" class="label__span span__number">Телефон*</span>
                            <input v-model="formDate.phone" @focus="hideLabel('phone')" @keypress="validatePhoneInput" @input="hideLabel('phone'), formatPhone, clearError('phone')" @blur="showLabel('phone')" class="form__input" id="tel" type="tel" >
                            <span v-if="errors.phone" class="error">{{ errors.phone }}</span>
                        </div>
                    </label>
                </div>

                <label class="form__textarea--message" for="message">
                    <span v-if="should.message" class="label__span span__textarea">Сообщение*</span>
                    <textarea v-model="formDate.message" @focus="hideLabel('message')" class="form__textarea" @input="hideLabel('message'), clearError('message')" @blur="showLabel('message')" id="message"></textarea>
                    <span v-if="errors.message" class="error-message">{{ errors.message }}</span>
                </label>

                <div class="form__send">
                    <label for="checkbox" class="form__checkbox">
                        <input v-model="formDate.agrement" class="checkbox__input" id="checkbox" type="checkbox" @input="clearError('agrement')" value="yes">
                        <span class="checkbox__span">Согласие на обработку персональных данных</span>
                        <span v-if="errors.agrement" class="error-agrement">{{ errors.agrement }}</span>
                    </label>
                    <div class="button">
                        <button id="project-button" type="submit" class="btn checkbox__btn">Обсудить проект</button>
                    </div>
                </div>

            </form>
        </div>
    </section>
</template>

<script setup>
    import { ref, onMounted } from 'vue';

    const formDate = ref( {
        name: '',
        email: '',
        phone: '',
        message: '',
        agrement: false,
    });

    const errors = ref({
        name: null,
        email: null,
        phone: null,
        message: null,
        agrement: null,
    });

    const should = ref({
        name: true,
        email: true,
        phone: true,
        message: true,
    })

    const hideLabel = (field) => {
        if(window.innerWidth <= 375 || window.innerWidth > 375 && window.innerWidth <= 768) {
            should.value[field] = false;
        }
    };

    const clearError = (field) => {
        errors.value[field] = '';
    };

    const showLabel = (field) => {
        if (!formDate.value[field]) {
            should.value[field] = true;
        }
    }

    const validatePhoneInput = (event) => {
        const charCode = (event.which) ? event.which : event.keyCode;
        const input = formDate.value.phone.replace(/\D/g, '');

        if (input.length >= 11 || (input.length === 0 && charCode !== 55)) {
            event.preventDefault();
        }

        if ((charCode < 48 || charCode > 57) || input.length >= 11){
            event.preventDefault();
        }
    };

    const validateName = () => {
        if (!formDate.value.name) {
            errors.value.name = 'Имя обязательно для заполнения';
        } else {
            errors.value.name = null;
        }
    };

    const validateEmail = () => {
        const re = /\S+@\S+\.\S+/;
        if (!formDate.value.email) {
            errors.value.email = 'Email обязателен для заполнения';
        } else if (!re.test(formDate.value.email)) {
            errors.value.email = 'Некорректный формат email';
        } else {
            errors.value.email = null;
        }
    };

    const formatPhone = () => {
        let input = formDate.value.phone.replace(/\D/g, ''); 
        if (input.length === 0) {
            input = '7';
        }
        else if (input.length > 0 && !input.startsWith('7')) {
            input = '7' + input.slice(1);
        }


        if (input.length > 11) {
            input = input.slice(0, 11);
        }

        formDate.value.phone = input;
    };

    const validatePhone = () => {
     const re = /^[0-9]{11}$/; 
        if (!formDate.value.phone) {
            errors.value.phone = 'Телефон обязателен для заполнения';
        } else if (!re.test(formDate.value.phone)) {
            errors.value.phone = 'Некорректный формат телефона';
        } else {
            errors.value.phone = null;
        }
    };

    const validateAgreement = () => {
        if(window.innerWidth <= 375) {
            formDate.value.agrement = true;
        } else {
            if (!formDate.value.agrement) {
                errors.value.agrement = 'Подтвердите согласие на обработку персональных данных'
            } else {
            errors.value.agrement = null;
                formDate.value.agrement = true;
            }
        }
    }

    const validateMessage = () => {
        if (!formDate.value.message) {
            errors.value.message = 'Расскажите о своём проекте, это обязательно'
        } else {
            errors.value.message = null;
        }
    }

    const resetForm = () => {
        formDate.value = {
            name: '',
            email: '',
            phone: '',
            message: '',
            agreement: false,
        };

        errors.value = {
            name: null,
            email: null,
            phone: null,
            message: null,
            agreement: null,
        };

        should.value = {
            name: true,
            email: true,
            phone: true,
            message: true,
        }

    };

    const submitForm = async () => {

        validateName();
        validateEmail();
        validatePhone();
        validateMessage();
        validateAgreement();
        formatPhone();
    
        if (!errors.value.name && !errors.value.email && !errors.value.phone && !errors.value.message && !errors.value.agrement) {

            const payload = {
                email: formDate.value.email,
                phone: formDate.value.phone,
                message: formDate.value.message,
            };

            console.log('Данные, которые вы отправили:', payload);

            try {
            const response = await fetch('https://api.test.cyberia.studio/api/v1/feedbacks', {
                method: 'POST',
                body: payload,
                headers: {
                    'Content-Type': 'application/json',
                }
                });

                if (!response) {
                    console.error('Ошибка при выполнении POST-запроса');
                } else {
                    console.log('Успешный POST-запрос:', response);
                    resetForm(); // сброс формы при успешной отправке
                }

            } catch (e) {
                console.error('Ошибка при выполнении POST-запроса:', e);
            }
        }
    }

    function updateButtonText () {
        const button = document.getElementById('project-button');
        if (window.innerWidth <= 375) {
            button.textContent = 'Отправить'; 
        } else {
            button.textContent = 'Обсудить проект';
        }
    }

    onMounted ( () => {
        window.addEventListener('resize', updateButtonText);
        window.addEventListener('load', updateButtonText);
    })


</script>

<style lang="scss">

    input:-webkit-autofill,
    input:-webkit-autofill:hover,
    input:-webkit-autofill:focus,
    input:-webkit-autofill:active {
        transition: background-color 5000s ease-in-out 0s;
        -webkit-text-fill-color: #fff;
    }


    // input[type=number]::-webkit-inner-spin-button,
    // input[type=number]::-webkit-outer-spin-button {
    //     -webkit-appearance: none;
    //     margin: 0;
    // }

    // input[type="text"]:focus, input[type="email"]:focus, input[type="number"]:focus {
    //     border-color: #2e76f9;
    // }
    
    

    // input[type="text"]:focus:valid, input[type="email"]:focus:valid {
    //     border-color: #2e76f9; 
    // }

    @mixin spanPosition {
        position: relative;
        top: 9px;
        left: 25px;
        padding: 0 5px;
        background: #2f2f3c;
        font-family: var(--font-family);
        font-weight: 300;
        font-size: 16px;
        color: #eff3ff;
        transition: 0.2s;
        pointer-events: none;
    }


    .feedback {
        &__title {
            margin-bottom: 72px;
            max-width: 385px;
            font-weight: 600;
            font-size: 40px;
            line-height: 56px;
            letter-spacing: 0.7px;
            color: #eff3ff;
        }
    }

    .form {
        position: relative;
        width: 100%;
        padding-bottom: 100px;

        &__block {
            position: relative;
            margin-bottom: 21px;
            display: flex;
            gap: 40px;
        }

        &__label {
            width: calc((100% - 80px) / 3);
            max-width: 100%;
        }

        &__input {
            width: 100%;
            padding: 21px 15px;
            border: 1.50px solid #9aa8ba;
            border-radius: 8px;
            font-family: var(--font-family);
            font-weight: 300;
            font-size: 16px;
            color: #eff3ff;
            transition: border 0.3s ease-in-out;

            &__input:focus {
                border: 2px solid #2e76f9; 
            }

            &__input:hover {
                border: 2px solid #2e76f9; 
            }

            &__input:active {
                border: 2px solid #2e76f9; 
            }
        }


        &__textarea {
            margin: 0;
            padding: 21px 15px;
            width: 100%;
            height: 139px;
            border: 1.50px solid #9aa8ba;
            border-radius: 8px;
            font-family: var(--font-family);
            font-weight: 300;
            font-size: 16px;
            color: #eff3ff;

            &__message {
                position: relative;
            }
        }

        &__send {
            padding-top: 39px;
        }

    }

    .label__span {
        @include spanPosition;
    }

    .span {
        &__name {
            padding: 0 15px;
        }

        &__email {
            padding: 0px 12px;
        }

        &__number {
            padding: 0px 12px;
        }
    }

    .span__textarea {
        position: relative;
        top: 10px;
        left: 26px;
        padding: 0 10px;
    }

    .checkbox {

        &__input {
            position: absolute;
            z-index: -1;
            opacity: 0;
        }

        &__span {
            cursor: pointer;
            display: flex;
            align-items: center;
            font-weight: 300;
            font-size: 16px;
            letter-spacing: 0.16px;
            color: #eff3ff;
        }

        &__input + &__span::before {
            content: '';
            display: inline-flex;
            width: 23px;
            height: 23px;
            flex-shrink: 0;
            flex-grow: 0;
            border: 1.50px solid #eff3ff;
            border-radius: 2px;
            margin-right: 10px;
            background-repeat: no-repeat;
            background-position: center center;
            transition:  border-color 0.3s ease-in-out; /* добавляем плавный переход */
            user-select: none;
        }
    
        &__input:checked + &__span::before {
            border-color: #2e76f9; /* изменяем цвет рамки при активации */
            background-image: url('data:image/svg+xml;utf8,<svg fill="%232e76f9" height="25" viewBox="0 0 20 20" width="25" xmlns="http://www.w3.org/2000/svg"><path d="M7.629 14.278l-3.748-3.75 1.133-1.133 2.614 2.614L15.5 5.464l1.133 1.133z"/></svg>'); /* добавляем изображение галочки */
        }
    
        &__input:focus + &__span::before {
            outline: 2px solid #2e76f9; /* добавляем outline при фокусе */
        }

        &__btn {
            --opacity: 1;
            --opacityColor: 1;
            cursor: pointer;
            border: 1px solid #2d76f9;
            border-radius: 85px;
            padding: 20px 55px;
            background: #2d76f9;
            font-weight: 400;
            font-size: 18px;
            color: #fff;
            opacity: var(--opacity);
            transition: opacity 0.3s ease-in-out, color 0.3s ease-in-out;

            &:focus {
                --opacity: 0.7;
            }

            &:hover {
                --opacity: 0.7;
            }
            &:active {
                --opacity: 0.9;
            }
        }

    }

    .error {
        left: 0px;
        bottom: 0px;
        position: relative;
        color: red;
        font-size: 0.875em;
    }

    .error-agrement {
        left: 0px;
        bottom: 0px;
        position: relative;
        color: red;
        font-size: 0.875em;
    }

    .error-message {
        left: 0px;
        bottom: 0px;
        position: relative;
        color: red;
        font-size: 0.875em;
    }

    .button {
        display: flex;
        justify-content: center;
        padding-top: 63px;
    }

    @media (max-width: 768px) {

        .form {
            padding-bottom: 80px;

            &__block {
                flex-direction: column;
            }

            &__label {
                position: relative;
                width: 100%;
            }
        }

        .label__span {
            position: absolute;
            font-weight: 400;
            font-size: 13px;
            color: #eef3ff;
            top: 20px;
            left: 0px;
            letter-spacing: 0.3px;
            background: none;
        }

        .span__textarea {
            top: -115px;
            left: 3px;
        }

    }

    @media (max-width: 375px) {

        .feedback {
            padding-bottom: 15px;

            &__title {
                margin-bottom: 34px;
                position: relative;
                padding-left: 103px;
                padding-top: 61px;
                font-weight: 500;
                font-size: 24px;
                color: #eef3ff;
                line-height: 29px;
                letter-spacing: 0.5px;

                &::before {
                    position: absolute;
                content: '';
                background-image: url('../img/planet.svg');
                width: 76px;
                height: 70px;
                left: 0px;
                bottom: -1px;
                }
            }
        }

        .form {
            &__input {
                padding: 16px 15px;
            }

            &__block {
                margin-bottom: 24px;
                gap: 22px;
            }

            &_label {
                position: relative;
            }
        }

        .label__span {
            position: absolute;
            font-weight: 400;
            font-size: 13px;
            color: #eef3ff;
            top: 20px;
            left: 0px;
            letter-spacing: 0.3px;
        }

        .span__textarea {
            top: -115px;
            left: 3px;
        }

        .checkbox__span {
            display: none;
        }

        .checkbox__btn {
            padding: 14px 125px;
            font-weight: 400;
            font-size: 15px;
            letter-spacing: 0.5px;
            color: #fff;
            }

        .form__send {
            padding-top: 38px;
        }

        .button {
            padding: 0;

            &::after {
                position: absolute;
                bottom: 16px;
                content: 'Нажимая “Отправить”, Вы даете согласие на обработку персональных данных';
                font-family: var(--second-family);
                font-weight: 400;
                font-size: 13px;
                text-align: center;
                line-height: 19px;
                color: #eef3ff;
            }
        }
    }

</style>
