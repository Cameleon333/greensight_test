<template>
<div class="main">
    <form action="" name="delivery-form" @submit="validation">
        <div class="group" :class="{ wide: smallScreen }" id="container-fio">
            <input type="text" id="fio" placeholder="Только кириллица" v-model="fio" :class="{error: isErrorFio}" @input="startInput">
            <label for="fio">ФИО</label>
            <p v-if="isErrorFio" class="err-message">{{ errorMessageFio }}</p>
        </div>
        <div class="group" :class="{ wide: smallScreen }">
            <input type="tel" id="phone" placeholder="+7 (___) ___-__-__" v-model="phone" :class="{error: isErrorPhone}" @input="startInput">
            <label for="phone">Телефон</label>
            <p v-if="isErrorPhone" class="err-message">{{ errorMessagePhone }}</p>
        </div>
        <div class="group wide">
            <input type="text" id="delivery-address" placeholder="Город, улица, дом" v-model="address" :class="{error: isErrorAddress}" @input="startInput">
            <label for="delivery-address">Адрес доставки</label>
            <p v-if="isErrorAddress" class="err-message">{{ errorMessageAddress }}</p>
        </div>
        <div class="group wide">
            <textarea name="comment" id="comment" class="comment" v-model="comment" :class="{error: isErrorComment}" @input="startInput"></textarea>
            <label for="comment" id="comment-label">Комментарий</label>
            <p v-if="isErrorComment" class="err-message">{{ errorMessageComment }}</p>
        </div>
        <div class="group wide button">
            <input type="submit" text="Отправить">
        </div>
    </form>
</div>
</template>

<script>
export default {
    name: 'Delivery',
    data() {
        return {
            isErrorFio: false,
            isErrorPhone: false,
            isErrorAddress: false,
            isErrorComment: false,
            errorMessageFio: '',
            errorMessagePhone: '',
            errorMessageAddress: '',
            errorMessageComment: '',
            maskFio: /^[А-Яа-яЁё-\s]+$/,
            fio: null,
            phone: null,
            address: null,
            comment: null,
            screenWidth: 0,
            smallScreen: false,
        }
    },

    methods: {
        validation(e) {
            let emptyMessage = "Поле обязательно для заполнения!";

            if (!this.maskFio.test(this.fio)) {
                this.isErrorFio = true;
                if (!this.fio) {
                    this.errorMessageFio = emptyMessage;
                } else {
                    this.errorMessageFio = "Допустимы только символы кириллицы, пробелы и тире!";
                }
            }
            if (!this.phone) {
                this.isErrorPhone = true;
                this.errorMessagePhone = emptyMessage;
            } else {
                if (this.phone.length > 18) this.phone = this.phone.slice(0, 18);
                else if (this.phone.length < 18) {
                    this.isErrorPhone = true;
                    this.errorMessagePhone = "Некорректный номер!";
                }
            }
            if (!this.address) {
                this.isErrorAddress = true;
                this.errorMessageAddress = emptyMessage;
            }
            if (!this.comment) {
                this.isErrorComment = true;
                this.errorMessageComment = emptyMessage;
            }

            if (!this.isErrorFio && !this.isErrorPhone && !this.isErrorAddress && !this.isErrorComment) {
                return true;
            }
            
            e.preventDefault(); 
        },

        startInput() {
            if (this.isErrorFio || this.isErrorPhone || this.isErrorAddress || this.isErrorComment) {
                switch(event.target.id) {
                    case("fio"):
                        this.isErrorFio = false;
                        break;
                    case("phone"):
                        this.isErrorPhone = false;
                        break;
                    case("delivery-address"):
                        this.isErrorAddress = false;
                        break;
                    case("comment"):
                        this.isErrorComment = false;
                        break;
                }
            }
        },

        setPhoneMask() {
            let element = document.getElementById('phone');
            let maskOptions = {
                mask: '+{7} (000) 000-00-00',
            };
            let mask = IMask(element, maskOptions);
        },

        updateScreenWidth() {
            this.screenWidth = window.innerWidth;
            if (this.screenWidth < 768) this.smallScreen = true;
            else this.smallScreen = false;
        }
    },

    mounted() {
        this.updateScreenWidth();
        this.setPhoneMask();
    },

    created() {
        window.addEventListener('resize', this.updateScreenWidth);
    }
}
</script>

<style lang="less" scoped>
    @font: 'Montserrat', sans-serif;
    @border-color: #b8bed8;
    @label-color: #3f3f3f;
    @input-disabled-color: #edeeef;
    @err-color: red;

    .main {
        font-family: @font;
        font-weight: 300;
        font-size: 14px;
    }

    label {
        color: @label-color;
        position: relative;
        bottom: 90px;
    }
    #comment-label {
        bottom: 165px;
    }
    
    input, .comment {
        display: inline-block;
        width: 100%;
        padding: 20px;
        border-radius: 3px;
        font-family: inherit;
        font-size: inherit;
        font-weight: inherit;
        border-style: none;
        border: 1px solid @border-color;
        margin-top: 10px;
        box-sizing: border-box;
        outline: none;
        cursor: pointer;
    }
    input::placeholder {
        color: @border-color;
    }
    input[type="tel"]::placeholder {
        letter-spacing: 2px;
    }
    input:hover, .comment:hover {
        border-color: @border-color - #111111;
    }
    input:focus, .comment:focus {
        border-color: @border-color - #222222;
    }
    input:focus::placeholder {
        font-size: 12px;
    }
    input:disabled, .comment:disabled {
        background-color: @input-disabled-color;
        border-color: @input-disabled-color;
        cursor: default;
        +label {
            color: @border-color;
        }
    }

    .group {
        display: inline-block;
        width: calc(50% - 20px);
        margin-top: 30px;
        white-space: pre-wrap;
    }

    #container-fio {
        margin-right: 40px;
        float: left;
    }

    .wide {
        width: 100%;
    }

    .comment {
        resize: none;
        height: 132px;
    }

    .button {
        text-align: right;
        margin-top: 0;
        min-width: 0;
    }

    .error {
        border-color: @err-color;
    }
    .error:hover {
        border-color: @err-color;
    }

    .err-message {
        color: @err-color;
        text-align: left;
        position: relative;
        bottom: 20px;
    }
</style>