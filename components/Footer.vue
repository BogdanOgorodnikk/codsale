<template>
    <footer class="footer">
        <div class="container">
            <h2 class="footer__headline">Бажаєте отримувати більше новин?</h2>
            <div class="footer__line"></div>
            <form class="footer__form" @submit.prevent="subscribe">
                <div class="footer__form-wrapper">
                    <h4 class="footer__form-wrapper--info">Email</h4>
                    <input type="text" class="footer__form-wrapper--input" placeholder="placeholder" v-model="email"
                        :class="{invalid: ($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email) || ($v.email.$dirty && !$v.email.maxLength)}"
                    >
                    <small 
                        class="helper-text__invalid"
                        v-if="$v.email.$dirty && !$v.email.required"
                    >Поле "Email" не повинно бути порожнім!</small>
                    <small 
                        class="helper-text__invalid"
                        v-else-if="$v.email.$dirty && !$v.email.email"
                    >Введіть коректний "Email"!</small>
                    <small 
                        class="helper-text__invalid"
                        v-else-if="$v.email.$dirty && !$v.email.maxLength"
                    >Максимальна кількість символів {{$v.email.$params.maxLength.max}} штук! Зараз їх: {{email.length}}</small>
                </div>
                <div class="footer__form-wrapper">
                    <h4 class="footer__form-wrapper--info">Ваше ім'я</h4>
                    <input type="text" class="footer__form-wrapper--input" placeholder="placeholder" v-model="name"
                        :class="{invalid: ($v.name.$dirty && !$v.name.required) || ($v.name.$dirty && !$v.name.minLength) || ($v.name.$dirty && !$v.name.maxLength)}"
                    >
                    <small 
                        class="helper-text__invalid"
                        v-if="$v.name.$dirty && !$v.name.required"
                    >Поле "Ваше ім'я" не повинно бути порожнім!</small>
                    <small class="helper-text__invalid"
                    v-else-if="$v.name.$dirty && !$v.name.minLength"
                    >Мінімальна кількість символів {{$v.name.$params.minLength.min}} штук! Зараз їх: {{name.length}}</small>
                    <small 
                        class="helper-text__invalid"
                        v-else-if="$v.name.$dirty && !$v.name.maxLength"
                    >Максимальна кількість символів {{$v.name.$params.maxLength.max}} штук! Зараз їх: {{name.length}}</small>
                </div>
                <button class="footer__form-button" type="submit">Підписатися</button>
            </form>
            <img class="footer__logo" src="../assets/logo.png" alt="footer-logo">
        </div>
    </footer>
</template>

<script>
import {email, required, minLength, maxLength} from 'vuelidate/lib/validators'

export default {
    data: () => ({
        email: '',
        name: '',
    }),
    validations: {
        email: {email, required, maxLength: maxLength(100)},
        name: {required,  minLength: minLength(2), maxLength: maxLength(100)},
    },
    methods: {
        subscribe() {
            if(this.$v.$invalid) {
                this.$v.$touch()
                return
            }
            const formData = {
                email: this.email,
                password: this.password,
                name: this.name,
                phone: this.phone
            }
            console.log(formData)
        }
    }
}
</script>

<style lang="scss" scoped>
    .footer {
        background: #363D41;
        overflow: hidden;
    }
    .footer__headline {
        font-family: 'Montserrat', sans-serif;
        font-weight: 800;
        font-size: 24px;
        color: #fff;
        margin-top: 41px;
    }
    .footer__line {
        width: 100%;
        height: 1px;
        background: #E5E5E5;
        margin-top: 8px;
    }
    .footer__form {
        display: flex;
        margin-top: 24px;
    }
    .footer__form-wrapper--info {
        font-family: 'Raleway', sans-serif;
        font-weight: 400;
        font-size: 12px;
        color: #fff;
    }
    .footer__form-wrapper--input {
        margin-top: 10px;
        border: 1px solid #C4C4C4;
        padding: 10px 15px 21px;
        background: inherit;
        color: #C4C4C4;
        font-family: 'Raleway', sans-serif;
        font-style: italic;
        font-weight: normal;
        font-size: 14px;
        width: 404px;
        margin-right: 64px;
        &::placeholder {
            color: #C4C4C4;
            font-family: 'Raleway', sans-serif;
            font-style: italic;
            font-weight: normal;
            font-size: 14px;
        }
    }
    .footer__form-button {
        display: block;
        background: #CC9966;
        border: 0px;
        color: #fff;
        font-family: 'Montserrat', sans-serif;
        font-weight: 800;
        font-size: 14px;
        text-align: center;
        width: 210px;
        height: 70px;
        margin-top: 18px;
        margin-left: 200px;
        cursor: pointer;
    }
    .footer__logo {
        display: block;
        margin: 49px auto 26px;
        width: 288px;
        height: 52px;
    }
    .invalid {
        border: 1px solid #b51818;
        color: #b51818;
        &::placeholder {
            color: #b51818;
        }
    }
    .helper-text__invalid {
        display: block;
        color: #b51818;
    }
    @media screen and (max-width: 320px) {
        .footer__headline {
            font-size: 18px;
        }
        .footer__line {
            margin-top: 16px;
        }
        .footer__form {
            display: block;
        }
        .footer__form-wrapper--input {
            max-width: 300px;
            width: 100%;
            margin-bottom: 17px;
        }
        .footer__form-button {
            margin: 24px auto 0;
            width: 175px;
            height: 45px;
            font-size: 12px;
        }
        .footer__logo {
            width: 204px;
            height: 38px;
        }
    }
</style>