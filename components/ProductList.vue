<template>
    <div class="product-list">
        <div class="container">
            <div class="product-list__box" v-for="product in paginatedData" :key="product.id">
                <h2 class="product-list__title" v-if="responsive">{{product.name}}</h2>
                <img class="product-list__images" :src="product.image" alt="product-images">
                <p class="product-list__type--info" v-if="responsive">
                    <img class="product-list__type--info-img" src="../assets/category.svg" alt="category"> 
                    <router-link class="product-list__type--info" v-for="category in product.categories" :key="category.id" :to="`/search ?category=${category.id}`">
                        {{category.name}},
                    </router-link>
                </p>
                <div class="product-list__item">
                    <h2 class="product-list__title" v-if="!responsive">{{product.name}}</h2>
                    <div class="product-list__price-block">
                        <p :class="{'price-discount': product.discount_amount}" class="product-list__price-block--standart-price">
                            <svg class="product-list__price-block--standart-dollar" width="17" height="12" viewBox="0 0 17 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path :class="{'price-discount-img': product.discount_amount}" d="M16.4993 0.601607C15.469 0.165804 14.4378 3.22167e-08 13.4072 3.22167e-08C10.1356 -0.000267825 6.86417 1.66982 3.59252 1.66982C2.77202 1.66982 1.95205 1.56482 1.13154 1.30232C1.03937 1.27286 0.946938 1.25893 0.856627 1.25893C0.399494 1.25893 0 1.61571 0 2.11098V10.609C0 10.9473 0.192044 11.2679 0.50043 11.3981C1.53077 11.8342 2.56191 12 3.59252 12C6.86417 12 10.1358 10.3299 13.4075 10.3299C14.228 10.3299 15.048 10.4349 15.8685 10.6974C15.9606 10.7269 16.0531 10.7408 16.1434 10.7408C16.6005 10.7408 17 10.384 17 9.88875V1.39098C16.9997 1.05241 16.8077 0.732054 16.4993 0.601607ZM1.27498 2.68446C1.80941 2.81946 2.36721 2.88723 2.94095 2.92366C2.78477 3.71089 2.0984 4.3042 1.27498 4.3042V2.68446ZM1.27498 10.3184V9.03857C2.18792 9.03857 2.92661 9.76902 2.96725 10.6832C2.36881 10.6347 1.80941 10.5142 1.27498 10.3184ZM8.49987 8.57143C7.32609 8.57143 6.3749 7.41991 6.3749 6C6.3749 4.57982 7.32635 3.42857 8.49987 3.42857C9.67338 3.42857 10.6248 4.57982 10.6248 6C10.6248 7.42045 9.67311 8.57143 8.49987 8.57143ZM15.7248 9.31554C15.2594 9.19795 14.7762 9.13205 14.2819 9.08946C14.4378 8.39089 15.0124 7.86054 15.7248 7.76946V9.31554ZM15.7248 2.99116C14.9043 2.88643 14.2676 2.19563 14.2423 1.3425C14.7613 1.40063 15.2541 1.50937 15.7248 1.68161V2.99116Z" fill="#CC9966"/>
                            </svg>
                            ${{product.price}}
                        </p>
                        <p class="product-list__price-block--discount-price" v-if="product.discount_amount"> <img class="product-list__price-block--discount-dollar" src="../assets/pricedollar.svg" alt="price">${{product.discount_amount}}</p>
                    </div>
                    <p class="product-list__describe">{{product.description}}</p>
                    <div class="product-list__type">
                        <p class="product-list__type--info" v-if="!responsive">
                            <img class="product-list__type--info-img" src="../assets/category.svg" alt="category"> 
                            <router-link class="product-list__type--info" v-for="category in product.categories" :key="category.id" :to="`/search ?category=${category.id}`">
                                {{category.name}},
                            </router-link>
                        </p>
                        <router-link  :to="`/_${product.id}`" class="product-list__type--link">переглянути</router-link>
                    </div>
                </div>
            </div>
            <div class="product-list__pagination">
                <button class="product-list__pagination--item" :class="{'cancel-pagination': pageNumber==0}" @click="prevPage" :disabled='pageNumber==0'>
                    Назад
                </button>
                <div class="product-list__pagination--item" :class="{'active-pagination': pageNumber==(index-1)}" v-for="index in pageCount" :key="index" @click="setPage(index-1)" v-if="Math.abs(index - pageNumber) < 3 || index == pageCount || index == 1">
                    {{index}}
                </div>
                <button class="product-list__pagination--item" :class="{'cancel-pagination': pageNumber >= pageCount -1}" @click="nextPage" :disabled='pageNumber >= pageCount -1'>
                    Вперед
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        products: [],
        pageNumber: 0,
        size: 5,
        responsive: false,
        windowWidth: window.innerWidth,
    }),
    mounted() {
        window.onresize = () => {
            this.windowWidth = window.innerWidth
            if(this.windowWidth <= 320) {
                this.responsive = true
            } else {
                this.responsive = false
            }
        }
        this.getProducts()
    },
    methods: {
        async getProducts() {
            const response = await fetch("https://gorest.co.in/public-api/products");
            const data = await response.json();
            this.products = data.data
        },
        nextPage() {
            this.pageNumber++
        },
        prevPage() {
            this.pageNumber--
        },
        setPage(page) {
            this.pageNumber = page;
        }
    },
    computed: {
        pageCount(){
            let l = this.products.length;
            let s = this.size;
            return Math.ceil(l/s);
        },
        paginatedData(){
            const start = this.pageNumber * this.size;
            const end = start + this.size;
            return this.products.slice(start, end);
        }
    }
}
</script>

<style lang="scss" scoped>
    .product-list {
        margin-top: 88px;
    }
    .product-list__box {
        display: flex;
        margin-bottom: 35px;
    }
    .product-list__images {
        width: 252px;
        height: 360px;
    }
    .product-list__item {
        border: 1px solid #E5E5E5;
    }
    .product-list__title {
        font-family: 'Montserrat', sans-serif;
        font-weight: 800;
        font-size: 18px;
        color: #363D41;
        margin-top: 54px;
        margin-left: 60px;
        max-width: 974px;
    }
    .product-list__price-block {
        display: flex;
        margin-left: 60px;
        margin-top: 12px;
    }
    .product-list__price-block--standart-price {
        color: #6C6C6C;
        font-family: 'Raleway', sans-serif;
        font-weight: 400;
        font-size: 12px;
    }
    .product-list__price-block--standart-dollar {
        margin-right: 10px;
    }
    .price-discount {
        text-decoration: line-through;
    }
    .price-discount-img {
        fill: #6C6C6C
    }
    .product-list__price-block--discount-price {
        color: #363D41;
        font-family: 'Raleway', sans-serif;
        font-weight: 700;
        font-size: 16px;
    }
    .product-list__price-block--discount-dollar {
        margin-left: 27px;
        margin-right: 10px;
    }
    .product-list__describe {
        font-family: 'Raleway', sans-serif;
        font-weight: 400;
        font-size: 14px;
        color: #363D41;
        max-width: 974px;
        margin-top: 28px;
        margin-left: 60px;
        line-height: 20px;
        max-height: 140px;
        height: 100%;
    }
    .product-list__type {
        margin-left: 60px;
        display: flex;
        justify-content: space-between;
    }
    .product-list__type--info {
        font-family: 'Raleway', sans-serif;
        font-weight: 500;
        font-size: 12px;
        color: #6C6C6C;
        text-decoration: none;
    }
    .product-list__type--info-img {
        margin-right: 10px;
    }
    .product-list__type--link {
        font-family: 'Montserrat', sans-serif;
        font-weight: 800;
        font-size: 14px;
        color: #363D41;
        margin-right: 60px;
        cursor: pointer;
        text-decoration: none;
        &::before {
            display: inline-block;
            width: 40px;
            height: 3px;
            background: #363D41;
            content: '';
            margin-right: 15px;
            margin-bottom: 3px;
        }
    }
    .product-list__pagination {
        display: flex;
        justify-content: center;
        margin-bottom: 141px;
        margin-top: 64px;
    }
    .product-list__pagination--item {
        font-family: 'Montserrat', sans-serif;
        font-weight: 600;
        font-size: 14px;
        color: #363D41;
        padding: 17px 21px 16px 20px;
        border: 1px solid #363D41;
        cursor: pointer;
        margin-right: 29px;
        background: inherit;
        &:hover {
            background: #CC9966;
            border: 1px solid #CC9966;
            color: #fff;
        }
        &:first-child {
            font-size: 12px;
        }
        &:last-child {
            font-size: 12px;
            margin-right: 0px;
        }
    }
    .active-pagination {
        background: #CC9966;
        border: 1px solid #CC9966;
        color: #fff;
    }
    .cancel-pagination {
        border: 1px solid #C4C4C4;
        color: #C4C4C4;
        pointer-events:none;
    }
    @media screen and (max-width: 320px) {
        .product-list {
            margin-top: 18px;
        }
        .product-list__box {
            display: block;
        }
        .product-list__images {
            display: block;
            max-width: 300px;
            width: 100%;
            height: 158px;
            margin-left: auto;
            margin-right: auto;
            margin-top: 16px;
        }
        .product-list__title {
            font-family: 'Montserrat', sans-serif;
            font-weight: 800;
            font-size: 16px;
            margin-top: 0px;
            margin-left: 0px;
            max-width: 300px;
        }
        .product-list__type--info {
            margin-top: 16px;
        }
        .product-list__item {
            border: 0px;
            border-bottom: 1px solid #E5E5E5;
        }
        .product-list__price-block {
            margin-left: 0px;
            margin-top: 16px;
        }
        .product-list__price-block--discount-price {
            font-size: 14px;
        }
        .product-list__describe {
            margin-left: 0px;
            margin-top: 14px;
            max-height: 100px;
            height: 100%;
            max-width: 300px;
            text-overflow: ellipsis;
            overflow: hidden;
            -webkit-line-clamp: 5;
            display: -webkit-box;
            -webkit-box-orient: vertical;
        }
        .product-list__type {
            display: block;
            margin-left: 0px;
            text-align: right;
            margin-bottom: 16px;
        }
        .product-list__type--link {
            margin-right: 0px;
            font-size: 12px;
            &::before {
                width: 20px;
                margin-right: 6px;
                margin-bottom: 2px;
                height: 2px;
            }
        }
        .product-list__pagination {
            margin-top: 22px;
            margin-bottom: 100px;
        }
        .product-list__pagination--item {
            font-size: 12px;
            padding: 5px 9px;
            margin-right: 12px;
            &:last-child {
                margin-right: 0px;
            }
        }
    }
</style>