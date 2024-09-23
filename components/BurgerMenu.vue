<template>
    <div class="menu">
        <input type="checkbox" id="burger-checkbox" class="burger-checkbox">
        <label for="burger-checkbox" id="label-burger" class="burger"></label>
        <div class="menu__list">
            <ul class="menu__items list-reset">
                <li><a href="#" class="menu__item">Агенство</a></li>
                <li><a href="#" class="menu__item">Услуги</a></li>
                <li><a href="#" class="menu__item">Кейсы</a></li>
                <li><a href="#" class="menu__item">Блог</a></li>
                <li><a href="#" class="menu__item">Контакты</a></li>
            </ul>
            <address class="address--burger">
                <ul class="address__items--burger list-reset">
                    <span class="address__item--span">Контакты:</span>
                    <li class="address__item--burger">
                        <a href="tel:+79991234567" class="address__link--burger address__phone">+7 999 123 45 67</a>
                    </li>
                    <li class="address__item--burger">
                        <a href="mailto: hello@cyberia.studio" class="address__link--burger address__email">hello@cyberia.studio</a>
                    </li>
                    <li class="address__item--burger">
                        <p class="address__descr--burger">ул.Ярных, д.35, оф.10</p>
                    </li>
                </ul>
            </address>
        </div>
    </div>
</template>

<script setup>

</script>

<style lang="scss">

    @mixin fontStyle {
        font-family: var(--second-family);
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        color: #9aa8ba;
        letter-spacing: 0.5px;
    }

    .menu {
        padding-right: 11px;
        padding-bottom: 20px;
    }

    .burger-checkbox {
        position: absolute;
        visibility: hidden;
    }

    .burger {
        position: relative;
        z-index: 101;
        cursor: pointer;
        display: block;
        border: none;
        background: transparent;
        width: 25px;
        height: 14px;
    }

    .burger::before,
    .burger::after {
        content: '';
        left: 0;
        position: absolute;
        display: block;
        width: 100%;
        height: 2px;
        border-radius: 10px;
        background: #eef3ff;
    }

    .burger::before {
        top: 0;
        box-shadow: 0 6px 0 #eef3ff;
        transition: box-shadow .3s .15s, top .3s .15s, transform .3s;
    }

    .burger::after {
        bottom: 0;
        transition: bottom .3s .15s, transform .3s;
    }

    .burger-checkbox:checked + .burger::before {
        top: 2px;
        transform: rotate(45deg);
        box-shadow: 0 6px 0 rgba(248, 248, 248, 0);
        transition: box-shadow .15s, top .3s, transform .3s .15s;
    }

    .burger-checkbox:checked + .burger::after {
        bottom: 11px;
        transform: rotate(-45deg);
        transition: bottom .3s, transform .3s .15s;
    }

    .menu__items {
        margin: 0;
        padding: 0;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        gap: 26px;
    }

    .menu__list {
        top: 0;
        left: 0;
        position: absolute;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        gap: 89px;
        padding: 82px 20px;
        margin: 0;
        background: #24252f;
        list-style-type: none;
        transform: translateX(-100%);
        transition: .3s;
        width: 100%;
        height: 100vh;
        z-index: 100;

        &::before {
            content: '';
            position: relative;
            left: 25px;
            bottom: -379px;
            border-radius: 20px;
            width: 303px;
            height: 2px;
            background: #2b2d3a;
        }

        &::after {
            content: '';
            position: relative;
            left: 25px;
            bottom: 35px;
            border-radius: 20px;
            width: 303px;
            height: 2px;
            background: #2b2d3a;
        }
    }
    
    .menu__item {
        display: block;
        padding: 4px;
        color: white;
        font-size: 18px;
        text-align: center;
        text-decoration: none;
    }

    .menu__item:hover {
        background: rgba(255,255,255,.2)
    }

    .burger-checkbox:checked ~ .menu__list {
        transform: translateX(0);
    }

    .address__item--span {
        padding-bottom: 20px;
        font-family: var(--second-family);
        font-style: normal;
        font-weight: 400;
        font-size: 18px;
        color: #9aa8ba;
    }

    .address__items--burger {
        padding: 0;
        display: flex;
        flex-direction: column;
        gap: 23px;
    }

    .address--burger {
        padding-left: 5px;
    }

    .address__link--burger {
        @include fontStyle;
    }

    .address__descr--burger {
        margin: 0;
        @include fontStyle;
    }

    @media(max-width: 376px) {
        .menu {
            padding: 0;

            &__list {
                top: 3px;
                padding-top: 0;
                gap: 75px;

                &::before {
                    left: 5px;
                    bottom: -373px;
                }

                &::after {
                    left: 5px;
                    bottom: 47px;
                }
            }

        }

        .burger {
            left: -9px;
            top: -5px;  
        }
    }
</style>