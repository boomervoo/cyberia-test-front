<script setup>

</script>

<template>
   <section class="crump">
        <div class="container">
            <div class="breadcrumb">
                <a class="breadcrumb__one" href="#">Главная</a>
                <span class="slash">&thinsp; /&thinsp;</span> 
                <a class="breadcrumb__two" href="#">Кейсы</a>
            </div>
        </div>
   </section>
</template>

<style lang="scss">


    .crump {
        padding-top: $paddingBreadCrump;
    }

    .breadcrumb {
        font-size: 14px;

        &__one {
            font-weight: 400;
            font-size: 16px;
            color: #9aa8ba;

            &:focus {
                outline-offset: 5px;
            }

            &__:hover {
                color: #fff;
            }
        }

        &__two {
            font-weight: 400;
            font-size: 16px;
            color: #C4D2E3;

            &:focus {
                outline-offset: 5px;
            }

            &__:hover {
                color: #fff;
            }
        }
    }
  

    .slash {
        font-size: 16px;
        color: #9AA8BA;
    }

    @media (max-width: 375px) {
        .breadcrumb {
            &__one {
                font-size: $fontSize;
            }

            &__two {
                font-weight: 300;
                color: #9aa8ba;
                font-size: $fontSize;
            }
        }

        .crump {
            padding-top: 45px;
        }
    }
  
</style>