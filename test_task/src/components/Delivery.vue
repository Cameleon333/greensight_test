<template>
<div class="main">
    <form action="" name="delivery-form">
        <div class="group" id="container-fio">
            <input type="text" id="fio" placeholder="Только кириллица" :pattern="maskFio">
            <label for="fio">ФИО</label>
        </div>
        <div class="group">
            <input type="tel" id="phone" placeholder="+7 (___) ___-__-__" :pattern="maskTel">
            <label for="phone">Телефон</label>
        </div>
        <div class="group wide">
            <input type="text" id="delivery-address" placeholder="Город, улица, дом">
            <label for="delivery-address">Адрес доставки</label>
        </div>
        <div class="group wide">
            <textarea name="comment" id="comment" class="comment"></textarea>
            <label for="comment" id="comment-label">Комментарий</label>
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
            maskTel: "+7 ([0-9]{3}([0-9]{3})([0-9]{2})([0-9]{2})",
            maskFio: "^[А-Яа-яЁё\\s]+$",
        }
    }
}
</script>

<style lang="less" scoped>
    @border-color: #b8bed8;
    @label-color: #3f3f3f;
    @input-disabled-color: #edeeef;

    .main {
        font-family: 'Montserrat', sans-serif;
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
        min-width: 220px;
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
        ~label {
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
    }
</style>