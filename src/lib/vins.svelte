<script lang="ts">
    import { onMount } from "svelte";
    import Swiper from "swiper"; //! Importé uniquement pour du typage
    import {register} from "swiper/element/bundle";
    register(true)

    import db from "../database/vins.json";
    import glass from "../assets/glass.svg"
    import bottle from "../assets/bottle.svg"

    export let locale = "fr-FR"
    export let currency = "EUR"

    let swiperElement: HTMLElement & {initialize: Function};
    let currentIndex: number = 3;

    function getSlideType(slideIndex: number, activeIndex: number): "odd" | "even" | "active" {
        if(slideIndex === activeIndex) return "active"
        
        const needToShift = slideIndex > currentIndex
        return (slideIndex - currentIndex + +needToShift) % 2 ? "even" : "odd"
                                          //`-> La magie de JS! (+true = 1; +false = 0) 
    }

    onMount(() => {
        const swiperParameters: typeof Swiper.defaults = {
            loop: true,
            slidesPerView: 3,
            autoHeight: false,
            preventInteractionOnTransition: true,
            autoplay: {
                delay: 5000,
                pauseOnMouseEnter: true
            },
            centeredSlides: true,
            spaceBetween: 10,
            longSwipes: false,
            initialSlide: currentIndex,
            slideToClickedSlide: true,
            on: {
                realIndexChange(swiper) {
                    currentIndex = swiper.realIndex
                }
            }
        }

        Object.assign(swiperElement, swiperParameters)
        swiperElement.initialize()
    })
</script>

<swiper-container
    class="slider"
    init="false"
    bind:this={swiperElement}
>
    <!-- Note: Je suis obligé de doubler `db` car sinon swiperjs bug un peu -->
    {#each Array(2).fill(db).flat() as item, index}
        {@const src = `/assets/img/vins/${item.img}`}
        <swiper-slide class="item" data-type="{getSlideType(index, currentIndex)}">
            <div class="image">
                <img {src} alt="{item.name} picture">
            </div>
            <div class="informations">
                <h3>{item.name}</h3>
                <div class="prices">
                    <div class="glass">
                        <img src={glass} alt="Glass illustration">
                        <p>{item.prices.glass.toLocaleString(locale, {style: "currency", currency, minimumFractionDigits: 2})}</p>
                    </div>
                    <div class="bottle">
                        <img src={bottle} alt="Bottle illustration">
                        <p>{item.prices.bottle.toLocaleString(locale, {style: "currency", currency, minimumFractionDigits: 2})}</p>
                    </div>
                </div>
            </div>
        </swiper-slide>
    {/each}
</swiper-container>

<style lang="scss">
    $img-skew: 5; // Valeur utilisé en deg et en pixel

    .slider {
        width: min(500px, 95%);
        margin: 0 auto;
        aspect-ratio: 16 / 9;

        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;

        .item {
            cursor: pointer;
            display: grid;
            grid-template-rows: 80% auto;
            grid-template-columns: 90%;
            justify-content: center;
            position: relative;
            height: 100%;
            padding-top: calc($img-skew * 3px);

            opacity: .5;
            transition: opacity .25s;
            
            .image {
                height: calc(100% - $img-skew * 3px);
                width: 100%;
                background-color: var(--color-green);
                transition: .25s;
                display: flex;
                align-items: center;
                justify-content: center;
                border-radius: 7.5px;
                padding: 5px;

                > img {
                    height: 100%;
                    width: auto;
                    -webkit-user-select: none;
                    -moz-user-select: none;
                    user-select: none;

                    transition: .25s;
                }
            }
            > .informations {
                display: flex;
                flex-direction: column;
                align-items: center;
                position: relative;
                > h3 {
                    display: inline-block;
                    position: relative;
                    left: 0;
                    translate: 0;
                    font-family: "coolvetica compressed";
                    transition: .25s;
                    font-size: 100%;
                }

                > .prices {
                    display: flex;
                    align-items: center;
                    justify-content: space-between;
                    transform-origin: bottom center;
                    scale: 0;
                    overflow: hidden;

                    transition: scale .25s;
                    
                    > * {
                        display: flex;
                        align-items: center;
                        justify-content: space-between;
                        font-size: 20px;
                        font-family: "coolvetica crammed";
                        width: 100%;
                        margin: 0 5px;
                    }
                }

            }
            
            @media screen and (max-width: 630px) {
                grid-template-rows: 75% auto;
                > .informations {
                    > h3 {
                        font-size: 20px
                    }
                    > .prices > * {
                        font-size: 15px;
                        > img {
                            height: 15px;
                        }
                    }
                }
            }
            @media screen and (max-width: 430px) {
                grid-template-rows: 70% auto;
                > .informations {
                    > h3 {
                        font-size: 15px;
                    }
                }
            }
            &[data-type="odd"] {
                > .image {
                    transform: skew(0, #{$img-skew}deg);
                }
            }
            &[data-type="even"] {
                > .image {
                    transform: skew(0, -#{$img-skew}deg);
                }
            }

            &[data-type="active"] {
                opacity: 1;
                cursor: grab;
                > .image {
                    height: 100%;
                    transform: skew(0);
                }
                > .informations {
                    > h3 {
                        translate: -50% 0;
                        left: 50%;
                    }
                    > .prices {
                        scale: 1;
                    }
                }
            }
        }
    }
</style>