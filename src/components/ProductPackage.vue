<template>
    <div class="package"
         :class="{
            package__selected: state.isSelected,
            package__disabled: !isAvailable,
            package__hover: state.isHover && isAvailable
            }"
    >
        <div class="package_card"
             @mouseenter="mouseEnter"
             @mouseleave="mouseLeave"
             @click="selectItem"
        >
            <div class="package_head">
                <div class="package_head-decor"></div>
            </div>
            <div class="package_body">
                <div class="package_content">
                    <transition name="fade" mode="out-in">
                        <span :key="1" class="package_claim" v-if="state.shouldDenied" style="color: #d91667;">Котэ не одобряет?</span>
                        <span :key="2" class="package_claim" v-else>Сказочное заморское яство</span>
                    </transition>
                    <h3 class="package_title">Нямушка</h3>
                    <span class="package_sort">{{ kind }}</span>
                    <ul class="package_discribe-list">
                        <li class="package_item"><b>{{ portions }}</b> порций</li>
                        <li class="package_item">{{ bonus }} в подарок</li>
                        <li class="package_item" v-if="isSatisfied">заказчик доволен</li>
                    </ul>
                </div>
                <div class="package_weight">
                    <span class="package_amount">{{ size }}</span>
                    <span class="package_unit">кг</span>
                </div>
            </div>
        </div>
        <transition name="fade" mode="out-in">
            <span :key="1" class="package_inform" v-if="!isAvailable">Печалька, {{ kind }} закончился.</span>
            <span :key="2" class="package_inform" v-else-if="state.isSelected">
                <span v-if="kind == 'с фуа-гра'">Печень утки разварная с артишоками.</span>
                <span v-if="kind == 'с рыбой'">Головы щучьи с чесноком да свежайшая сёмгушка.</span>
                <span v-if="kind == 'с курой'">Филе из цыплят с трюфелями в бульоне.</span>
            </span>
            <span :key="3" class="package_inform" v-else>Чего сидишь? Порадуй котэ,
                <a href="#" @click.prevent="selectItem">купи.</a>
            </span>
        </transition>
    </div>
</template>

<script>
    export default {
        name: 'ProductPackage',
        props: {
            kind: String,
            size: Number,
            isSatisfied: Boolean,
            isAvailable: Boolean
        },
        data() {
            return {
                title: 'Нямушка',
                state: {
                    isSelected: false,
                    isHover: false,
                    shouldDenied: false
                }
            }
        },
        computed: {
            portions() {
                return this.size * 20;
            },
            bonus() {
                var bonusText;
                switch (this.size) {
                    case 0.5:
                        bonusText = 'мышь';
                        break;
                    case 2:
                        bonusText = '2 мыши';
                        break;
                    case 5:
                        bonusText = '5 мышей';
                        break;
                }
                return bonusText;
            }
        },
        methods: {
            selectItem() {
                if (this.isAvailable) this.state.isSelected = !this.state.isSelected;
                if(!this.state.isSelected) this.state.shouldDenied = false;
            },
            mouseEnter() {
                this.state.isHover = !this.state.isHover;
                if (this.state.isSelected && this.state.isHover) this.state.shouldDenied = !this.state.shouldDenied;
            },
            mouseLeave() {
                this.state.isHover = !this.state.isHover;
                this.state.shouldDenied = false;
            }
        }
    }
</script>

<style lang="css">
    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>

<style scoped lang="scss">
    .package {
         display: inline-block;
         margin: 0 4rem 3rem;
         font-family: 'Trebuchet MS';

    &_card {
         width: 32rem;
         margin: 0 auto 1.5rem;
         position: relative;
         overflow: hidden;
         text-align: left;
         cursor: pointer;
     }

    &_head {
         height: 5rem;
         transform: skewX(-45deg);
         transform-origin: 0 100%;
         border-left-style: solid;
         border-left-color: #1698d9;
         border-left-width: .5rem;
         transition: all 400ms ease;
         background-image: linear-gradient(to right, rgba(242, 242, 242, 1) 0%, rgba(242, 242, 242, 1) 50%, rgba(242, 242, 242, 0) 50%, rgba(242, 242, 242, 0) 100%);

        &-decor {
             margin-left: 2rem;
             padding-left: 1rem;
             padding-top: 2rem;
             height: 5rem;
             width: 27rem;
             z-index: 2;
             transform: skewX(45deg);
             border-top-style: solid;
             border-top-color: #1698d9;
             border-top-width: .4rem;
             border-right-style: solid;
             border-right-color: #1698d9;
             border-right-width: .4rem;
             border-radius: 0 1.2rem 0 0;
             background-color: #f2f2f2;
             transition: all 400ms ease;
         }
     }

    &_body {
         height: 43rem;
         margin-top: -.1rem;
         border-bottom-style: solid;
         border-bottom-color: #1698d9;
         border-bottom-width: .4rem;
         border-right-style: solid;
         border-right-color: #1698d9;
         border-right-width: .4rem;
         border-left-style: solid;
         border-left-color: #1698d9;
         border-left-width: .4rem;
         border-radius: 0 0 1.2rem 1.2rem;
         background-color: #f2f2f2;
         transition: all 400ms ease;
     }

    &_content {
         padding-left: 5rem;
         position: relative;
         top: -3rem;
         height: calc(100% + 3rem);
         border-radius: .8rem;
         background-repeat: no-repeat;
         background-position: 0 100%;
         background-image: url('../assets/img/bg_cat.png');
     }

    &_claim {
         font-size: 1.6em;
         font-weight: 400;
         color: #666;
     }

    &_title {
         font-size: 4.6em;
         font-weight: 700;
         color: black;
     }

    &_sort {
         color: black;
         font-size: 2.4rem;
         font-weight: 700;
     }

    &_discribe-list {
         margin-top: 1rem;
         color: #666;
         font-size: 1.4em;
         line-height: 1.6rem;
         font-weight: 400;
     }

    &_inform {
         text-shadow: 0 0.1rem 0 black;
         font-size: 1.3em;
         font-weight: 400;
         line-height: 1.6rem;
         color: white;

        a {
            color: #1698d9;
            transition: all 400ms ease;
            text-decoration: underline #1698d9 dashed;
        }
     }

    &_weight {
         width: 8rem;
         height: 8rem;
         position: absolute;
         right: 1.5rem;
         bottom: 1.5rem;
         padding-top: 2rem;
         border-radius: 50%;
         text-align: center;
         color: white;
         background-color: #1698d9;
         transition: all 400ms ease;
     }

    &_amount {
         display: block;
         font-weight: 400;
         line-height: 2.2rem;
         font-size: 4.2em;
     }

    &_unit {
         font-size: 2.1rem;
     }
    }

    .package__hover {

        .package_head {
            border-left-color: #2ea8e6;

        &-decor {
             border-top-color: #2ea8e6;
             border-right-color: #2ea8e6;
         }
        }

        .package_body {
            border-left-color: #2ea8e6;
            border-bottom-color: #2ea8e6;
            border-right-color: #2ea8e6;
        }

        .package_weight {
            background-color: #2ea8e6;
        }

        .package_inform {
            a {
                color: #2ea8e6;
                text-decoration: underline #2ea8e6 dashed;
            }
        }
    }

    .package__selected {

        .package_head {
             border-left-color: #d91667;

            &-decor {
                 border-top-color: #d91667;
                 border-right-color: #d91667;
             }
         }

        .package_body {
             border-left-color: #d91667;
             border-bottom-color: #d91667;
             border-right-color: #d91667;
         }

        .package_weight {
             background-color: #d91667;
         }
    }

    .package__selected.package__hover {

        .package_head {
            border-left-color: #e62e7a;

        &-decor {
             border-top-color: #e62e7a;
             border-right-color: #e62e7a;
         }
        }

        .package_body {
            border-left-color: #e62e7a;
            border-bottom-color: #e62e7a;
            border-right-color: #e62e7a;
        }

        .package_weight {
            background-color: #e62e7a;
        }
    }

    .package__disabled {

        .package_card {
            cursor: inherit;
        }

        .package_head {
            border-left-color: #b3b3b3;

            &-decor {
                 border-top-color: #b3b3b3;
                 border-right-color: #b3b3b3;
             }
        }

        .package_body {
            border-left-color: #b3b3b3;
            border-bottom-color: #b3b3b3;
            border-right-color: #b3b3b3;
        }

        .package_inform {
            color: white;
        }

        .package_weight {
            background-color: #b3b3b3;
        }

        .package_content {
            opacity: .5;
         }

        .package_ref {
            cursor: inherit;
         }

        .package_inform {
            color: #ff6;
         }
    }

    @media screen and (max-width: 359px) {
        .package {
            &_card {
                 width: 30rem;
             }

            &_head {

                &-decor {
                     width: 25rem;
                 }
            }
        }
    }
</style>
