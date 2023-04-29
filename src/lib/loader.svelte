<script lang="ts">
    import gsap, {Elastic, Power1, Power3} from "gsap"
    import {onMount} from "svelte"

    export let loading = true;
    export let fullscreen= true;
    export let scale = 1;

    export let autoStartOnMount= true;
    export let then: Function = () => null;

    const animationDuration = 2//s
    const entryAnimationTiming = 10//%
    const maxStretch = 1.15
    const delayBetweenStretches = .5//s

    const entryAnimationDuration = animationDuration * entryAnimationTiming/100

    const endingAnimationDuration = 1//s

    const exitAnimationDuration = .5//s

    let image: Element;
    let text: Element;
    let container: Element;
    let loaderEl: Element;

    let initialParentStyle = ""
    function exitAnimation() {
        gsap.to(container, {
            scale: 15,
            opacity: 0,
            delay: exitAnimationDuration / 2,
            ease: Power3.easeIn,
            duration: exitAnimationDuration
        })
        gsap.to(loaderEl, {
            opacity: 0,
            ease: Power1.easeInOut,
            delay: exitAnimationDuration / 1.5,
            duration: exitAnimationDuration
        }).then(() => {
            if(initialParentStyle) loaderEl.parentElement.setAttribute("style", initialParentStyle)
            else loaderEl.parentElement.removeAttribute("style")
            loaderEl.remove()
            then()
        })
    }

    function endAnimation() {
        gsap.fromTo(image, {transformOrigin: "center center"}, {
            rotate: -90,
            duration: endingAnimationDuration / 5,
            ease: Power1.easeInOut
        }).then(() => {
            text.setAttribute("style", "display: unset;")
            gsap.to(text.children, {
                opacity: 1,
                stagger: {
                    amount: endingAnimationDuration /2,
                    from: "end"
                },
            })
            gsap.to(text, {
                translateX: 0,
                duration: endingAnimationDuration,
                ease: Power1.easeInOut
            })
            gsap.to(container, {
                translateX: "-50%",
                duration: endingAnimationDuration,
                ease: Power1.easeInOut
            }).then(() => {
                gsap.fromTo(image, {zIndex: 0}, {
                    rotate: 0,
                    duration: endingAnimationDuration / 5,
                    ease: Power1.easeInOut
                })
            }).then(exitAnimation)
        })
    }

    function startAnimation() {
        if(!(
            image
            && text
            && container
            && loaderEl
        )) return;
        gsap.to(image, {
            keyframes: [
                {
                    scaleY: maxStretch,
                    ease: null,
                    duration: entryAnimationDuration
                }, {
                    scaleY: 1, 
                    ease: Elastic.easeOut,
                    duration: animationDuration - entryAnimationDuration
                },
                {duration: delayBetweenStretches}
            ]
        }).then(() => {
            if(loading) return startAnimation()
            else return endAnimation()
        })
    }
    
    export function animate() {
        gsap.to(container, {opacity: 1, delay: .5}).then(() => {
            if(loading) return startAnimation()
            else return endAnimation()
        })
    }

    onMount(() => {
        initialParentStyle= loaderEl.parentElement.getAttribute("style")
        loaderEl.parentElement.setAttribute("style", "max-height: 100vh; overflow-y: hidden;")
        if(autoStartOnMount) animate()
    })
</script>

<div class="loader" class:fullscreen bind:this={loaderEl}>
    <div class="container" style="scale: {scale};transform: translateX(-{(1 - scale) * 50}%)" bind:this={container}>
        <!-- Pour éviter un surplus de chargement, j'évite de faire un requête de plus pour l'image de chargement -->
        <div class="icon" bind:this={image}>
            <svg viewBox="0 0 150 150" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M108.039 89.1265C104.914 97.009 103.247 112.594 106.372 112.774C106.581 95.7565 108.039 89.1265 108.039 89.1265ZM100.219 61.7315C100.219 61.7315 91.9664 79.649 91.7242 100.274C91.6364 107.774 91.7319 112.774 91.7319 112.774H94.2242C94.2242 112.774 93.9204 81.649 100.219 61.7315ZM77.2457 112.774C77.2457 112.774 75.7867 61.7315 69.1207 48.6065C69.1207 48.6065 74.8864 88.604 74.3989 112.774C76.2037 112.774 77.2457 112.774 77.2457 112.774ZM53.5524 64.024C53.5524 64.024 57.9274 85.899 57.7184 100.274C57.5094 107.774 57.7262 112.774 57.7262 112.774H60.2184C60.2184 112.774 62.7194 79.439 53.5524 64.024ZM38.9117 85.274C38.9117 85.274 40.3697 92.979 40.5787 112.774C43.7037 112.564 42.0367 94.439 38.9117 85.274ZM148.391 50.5865C144.92 74.5165 118.64 74.9791 118.64 74.9791L118.185 119.104H25.6852L25.4097 75.5341C25.4097 75.5341 3.65486 71.5816 0.783759 58.0865C-1.21819 48.6765 -0.079515 34.7715 12.5172 25.5865C33.2984 10.429 51.6724 28.0565 51.6724 28.0565C51.6724 28.0565 51.7769 21.0865 38.4654 16.6365C44.6412 4.64903 76.5162 -6.60097 94.1227 10.949C108.648 25.4315 109.188 37.989 106.528 45.9615C113.185 38.449 112.469 24.439 103.654 14.8565C128.498 8.29401 152.218 24.199 148.391 50.5865ZM25.5807 121.784H118.289V143.034H25.5807V121.784Z" fill="url(#paint0_linear_1_298)"/>
                <defs>
                <linearGradient id="paint0_linear_1_298" x1="74.3996" y1="143.034" x2="74.3996" y2="1.87498" gradientUnits="userSpaceOnUse">
                <stop stop-color="#59114D"/>
                <stop offset="1" stop-color="#E98A15"/>
                </linearGradient>
                </defs>
            </svg>                
        </div>
        <p bind:this={text}>
            {#each "TOKIEST" as letter}
                <span class="letter">{letter}</span>
            {/each}
        </p>
    </div>
</div>

<style lang="scss">
    $backgroundColor: var(--color-darkgreen);

    .loader {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        &.fullscreen {
            height: 100vh;
        }
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;

        overflow: hidden;
        z-index: 999999999;

        background-color: $backgroundColor;

        &, & * {
            will-change: transform;
        }

        $imageW: min(25vw, 250px);
        > .container {
            opacity: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            position: absolute;
            top: 50%;
            left: 50%;
            translate: calc(calc($imageW * -1) / 2) -50%;

            > .icon {
                transform-origin: center bottom;
                height: $imageW;
                width: $imageW;
                z-index: 1;
                margin-right: 25px;
                background-color: $backgroundColor;
                svg {
                    height: 100%;
                    width: 100%;
                }
            }
            > p {
                translate: -100% 0;
                color: var(--color-white);
                font-family: "coolvetica compressed";
                font-size: calc($imageW * .90);
                letter-spacing: 10px;
                display: none;
                > span {
                    opacity: 0;
                }
            }
        }
    }
</style>