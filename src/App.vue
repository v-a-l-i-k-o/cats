<template>
    <div id="app">
        <h1 class="title">Ты сегодня покормил кота?</h1>
        <div class="package-list">
            <div class="package-list_row">
                <template v-for='product in products'>
                    <ProductPackage
                            v-for='(item, index) in product.discribe'
                            :key="product.alias + index"
                            :kind="product.alias"
                            :size="item.size"
                            :isSatisfied="item.isSatisfied"
                            :isAvailable="item.available"
                    ></ProductPackage>
                </template>
            </div>
        </div>
    </div>
</template>

<script>

    import ProductPackage from './components/ProductPackage.vue';

    export default {
        name: 'app',
        data() {
            return {
                products: {
                    foieGras: {
                        alias: 'с фуа-гра',
                        discribe: [{
                            size: 0.5,
                            isSatisfied: false,
                            available: true
                        },

                        ]
                    },
                    fish: {
                        alias: 'с рыбой',
                        discribe: [{
                            size: 2,
                            isSatisfied: false,
                            available: true
                        }
                        ]
                    },
                    chiken: {
                        alias: 'с курой',
                        discribe: [{
                            size: 5,
                            isSatisfied: true,
                            available: false
                        }
                        ]
                    },
                }
            }
        },
        components: {
            ProductPackage
        }
    }
</script>

<style lang="scss">

    @font-face {
        font-family: 'Exo2.0-Thin';
        src: url('./assets/fonts/Exo2.0-Thin.otf');
    }

    #app {
        min-height: 100vh;
        font-family: 'Exo2.0-Thin', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        padding: 1.5rem;
        background-image: linear-gradient(to top, rgba(0, 0, 0, 0.25) 0%, transparent 50%, rgba(0, 0, 0, 0.25) 100%),
        url('./assets/img/bg.png');
    }

    h1 {
        margin-bottom: 2rem;
        text-align: center;
        font-size: 3.6em;
        font-weight: 100;
        color: white;
        text-shadow: 0 0.1rem 0 black;
    }

    .package-list {
        margin: 0 auto;
        max-width: 112rem;
        text-align: center;

    &_row {
        margin: 0 -4rem;
    }

    }

    @media screen and (max-width: 359px) {
        #app {
            padding: 1rem;
        }
    }
</style>
