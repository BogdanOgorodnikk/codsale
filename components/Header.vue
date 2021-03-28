<template>
    <header class="header">
        <div class="container">
            <img class="header__logo" src="../assets/logo.png" alt="header">
        </div>
    </header>
</template>

<style lang="scss" scoped>
    .header {
        background: #363D41;
        overflow: hidden;
    }
    .header__logo {
        display: block;
        margin: 20px auto;
    }
    @media screen and (max-width: 320px) {
        .header__logo {
            width: 204px;
            height: 38px;
        }
    }
</style>