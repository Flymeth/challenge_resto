<script lang="ts">
	import { onMount } from "svelte";
    import logo from "../../assets/logo.svg"
    import mouse from "../../assets/mouse.svg"
    import gsap from "gsap";
    import scrollTrigger from "gsap/ScrollTrigger"
    import AllDays from "./pages/allDays.svelte";
    import Today from "./pages/today.svelte";

    export let maxMenuWidth = 849;//px
    export let mobileNavigation: boolean | undefined = undefined;
    export let customPages = [AllDays, Today];
    export let scrollViewOffset = 65; // = hauteur de la navbar
    const scrollerViewHeight = (window.innerHeight + scrollViewOffset) /2

    gsap.registerPlugin(scrollTrigger)
    scrollTrigger.normalizeScroll(!navigator.userAgent.includes("Mobile")) // Sur mobile, le carousel fait bugger ce truc

    let menu: HTMLElement;
    let menuPages: HTMLOListElement;

    function startMagic() {
        if(mobileNavigation === undefined) mobileNavigation = menu.clientWidth < maxMenuWidth
        window.addEventListener('resize', () => {
            console.warn("Hey! Tu viens de changer la taille de la fenêtre! Malheureusement, le code ne permet pas de directement actualiser la page de la version PC à la version Mobile (et inversement)!")
            console.log("Pour actualiser entièrement la responsivité du site, il te faut recharger la fenêtre!")
        })

        const scene = gsap.timeline({
            scrollTrigger: {
                trigger: menuPages,
                pin: true,
                start: `center ${scrollerViewHeight}px`,
                scrub: 1.5,
                fastScrollEnd: true
            }
        })

        for(const i in Array.from(menuPages.children)) {
            const index = parseInt(i)
            const movingPage= menuPages.children[index]
            if(mobileNavigation) {
                if(index === menuPages.children.length -1) break;
                scene.to(movingPage, {
                    translateY: "+=50%",
                    translateX: "+=20%",
                    rotateZ: -40,
                    opacity: 0,
                })
            }else if(index % 2 === 0) {
                const nextPage  = menuPages.children[index + 1]
                if(!nextPage) break;

                scene
                .to(movingPage, {
                    rotateY: -90
                })
                .from(nextPage, {
                    rotateY: 90
                })
            }
        }
    }

    onMount(() => {
        if(menu && menuPages) startMagic()
    })
</script>

<div class="menu-container" style="--minMenuWidth: {maxMenuWidth}px;--maxPageZindex: {customPages.length}" bind:this={menu} class:mobile={mobileNavigation}>
    <ol class="pages menu-card" bind:this={menuPages}>
        <li class="cover" style="z-index: {customPages.length + 1};">
            <img src={logo} alt="Our logo">
            <h3>Découvrez notre carte</h3>
            <img src={mouse} alt="A moving mouse to make you scroll" class="call-to-action_scrolling">
        </li>
        {#each customPages as Page, index}
            <li style="z-index: {index % 2 || mobileNavigation ? customPages.length - index : index};">
                <Page />
                <span class="page-number">{index + 1}</span>
            </li>
        {/each}
        <li class="back">
            <h3>Bon appétit!</h3>
        </li>
    </ol>
</div>

<style lang="scss">
    .menu-container {
        $minMenuWidth: var(--minMenuWidth); // La variable --minMenuWidth est définie directement dans le HTML
        $menuCardW: min($minMenuWidth, 100%);
        $menuCardRatio: #{1 / .7};
        $mobileCardRation: 1 / calc($menuCardRatio);
        $pageShadowW: 25px;

        $page-border-radius: 15px;
        $paddingCalculation: calc($page-border-radius / 2);
        $page-number-offset: 10px;
        $mobile-clipper-borderW: 10px;

        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;
        overflow: hidden;
        width: 100%;

        // --- UTILISATEUR (GLOBAL)
        ol.menu-card {
            list-style: none;
            position: relative;
            border-radius: 15px;
            margin: 0 auto;
            width: $menuCardW;
            max-height: calc(100vh - 80px); // navbar's height = 65px
            > li {
                background-color: var(--color-white);
                padding: $paddingCalculation;
                overflow: hidden;
                will-change: transform;
                position: absolute;
                top: 50%;
                height: 100%;
                translate: 0 -50%;

                &::after { // Pour éviter d'avoir des couches visibles en dessous des autres (et d'avoir la couleur approprié sans ajouter une autre couleur)
                    content: "";
                    position: absolute;
                    top: 0;
                    left: 0;
                    width: 100%;
                    height: 100%;
                    z-index: -2;
                    background-color: rgba(var(--rgb-orange), .25);
                }

                > .page-number {
                    position: absolute;
                    bottom: 5px;
                    font-size: 15px;
                }

                &:first-of-type, &:last-of-type {
                    display: flex;
                    flex-direction: column;
                    align-items: center;
                    justify-content: space-evenly;
                    color: var(--color-orange);
                    background: url("../../assets/images/lether_texture.png");
                    padding: $paddingCalculation;
                    text-transform: capitalize;
                    word-spacing: 5px;

                    &::after { // Cette propriété a déjà été "instancié"
                        background: url("../../assets/images/lether_shadow.png");
                        opacity: .35;
                        z-index: 1;
                    }

                    > img {
                        width: 100%;
                        &.call-to-action_scrolling {
                            position: absolute;
                            bottom: 5px;
                            width: min(50px, 20%);

                            left: 50%;
                            translate: -50% 0;

                            animation: mouseAnim 2s infinite alternate ease-in-out;

                            @keyframes mouseAnim {
                                from {
                                    translate: -50% 0;
                                }
                                to {
                                    translate: -50% -10px;
                                }
                            }
                        }
                    }

                    > h3 {
                        filter: brightness(.85);
                        font-size: min(45px, calc(7.5vw / $menuCardRatio));
                    }
                }
            }
        }

        // --- UTILISATEUR <MOBILE>
        &.mobile {
            ol.menu-card {
                aspect-ratio: $mobileCardRation;
                > li {
                    width: 100%;
                    border-radius: $page-border-radius;

                    > .page-number {
                        right: $page-number-offset;
                    }
                }

                &::before { // clipper
                    content: "";
                    position: absolute;
                    top: 0;
                    left: 50%;
                    height: 2.5%;
                    width: 30%;
                    translate: -50% 0;
                    border: groove var(--color-white);
                    border-width: 0 $mobile-clipper-borderW $mobile-clipper-borderW;
                    opacity: .75;
                    box-shadow: 0 0 10px rgba(var(--rgb-white), .75);

                    z-index: calc(var(--maxPageZindex) + 2); // Définie directement dans le HTML (+2 car il y a la cover et le back)
                }
            }
        }

        // --- UTILISATEUR <PC>
        &:not(.mobile) {
            ol.menu-card {
                aspect-ratio: $menuCardRatio;
                perspective: 6000px;
                transform-style: preserve-3d;
                > li {
                    width: 50%;

                    &:nth-child(even) { // = pages de gauche
                        right: 50%;
                        transform-origin: right center;
                        border-radius: $page-border-radius 0 0 $page-border-radius;
                        padding-right: $paddingCalculation + calc($pageShadowW / 2);
    
                        > .page-number {
                            left: $page-number-offset;
                        }
                        
                        &::before {
                            content: "";
                            position: absolute;
                            top: 0;
                            right: 0;
                            height: 100%;
                            width: $pageShadowW;
    
                            background: linear-gradient(to left, rgba(var(--rgb-black), .5), transparent);
                        }
                    }
                    &:nth-child(odd) { // = pages de droite
                        left: 50%;
                        transform-origin: left center;
                        border-radius: 0 $page-border-radius $page-border-radius 0;
                        padding-left: $paddingCalculation + calc($pageShadowW / 2);
    
                        > .page-number {
                            right: $page-number-offset;
                        }
    
                        &:not(:last-of-type)::before {
                            content: "";
                            position: absolute;
                            top: 0;
                            left: 0;
                            height: 100%;
                            width: $pageShadowW;
    
                            background: linear-gradient(to right, rgba(var(--rgb-black), .5), transparent);
                        }
                    }
                }
            }

        }
    }
</style>