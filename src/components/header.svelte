<script lang="ts">
    import gsap, {Expo} from "gsap"
    import {onMount} from "svelte"

    export let title = "Un restaurant où les chefs sont plus que de simples employés"
    export let accentBorn = [32, 36]
    export let lineBreaks = [26]
    export let autoStartOnMount = true
    const beforeContent = Array.from(title).map((l, i) => lineBreaks.includes(i) ? "\n" : l).join('')

    export let animationDuration = 1.25//s

    export let titleAnimation = true;
    
    let h1: HTMLHeadingElement;
    let header: HTMLElement;
    export function animate() {
        const h1ChildrenToAnimate: HTMLSpanElement[] = [...h1.children].filter((e) : e is HTMLSpanElement => e.nodeName === "SPAN" && !e.classList.contains('space'))
        // Il y a également des <br /> et des <span></span> utilisé uniquement pour les espaces (qui ne sont pas à animer)

        gsap.to(h1ChildrenToAnimate, {
            opacity: 1,
            translateY: 0,
            ease: Expo.easeOut,
            stagger: {
                amount: animationDuration,
                from: "start"
            },
        })
        
        h1ChildrenToAnimate.forEach(e => {
            let isAnimating= false
            e.addEventListener('mouseover', () => {
                if(isAnimating) return;

                isAnimating = true
                gsap.to(e, {
                    "--translateY": 0,
                    repeat: 1,
                    yoyo: true,
                    duration: .25
                }).then(() => isAnimating = false)
            })
        })

        if(titleAnimation) document.addEventListener('mousemove', (event) => {
            const {x, y} = event
            const offsetX = x - header.clientWidth/2
            const offsetY = y - header.clientHeight/2

            const pourcentageOffsetX = offsetX / 25
            const pourcentageOffsetY = offsetY / 25
            gsap.to(header, {
                "--translateOffsetX": -pourcentageOffsetX,
                "--translateOffsetY": -pourcentageOffsetY,
                duration: 2
            })
        })
    }

    if(autoStartOnMount) onMount(animate)
</script>

<header bind:this={header}>
    <h1 data-title={beforeContent} bind:this={h1}>
        {#each title as letter, i}
            {#if lineBreaks.includes(i)}
                <br />
            {/if}

            {@const accent = accentBorn[0] <= i && i <= accentBorn[1]}
            {#if letter === " "}
                <span class="space" data-index={i} class:accent></span>
            {:else}
                <span style="z-index: {title.length - i};" data-letter={letter} data-index={i} class="letter" class:accent>{letter}</span>
            {/if}
        {/each}
    </h1>
</header>

<style lang="scss">
    header {
        height: 100vh;
        position: relative;
        overflow: hidden;
        display: flex;
        align-items: center;
        justify-content: flex-end;
        padding: 5px 25px;

        $bg-blur: 5px;
        --translateOffsetX: 0px;
        --translateOffsetY: 0px;
        &::before {
            content: "";
            position: absolute;
            top: 50%;
            left: 50%;
            translate: -50% -50%;
            width: calc(100% + $bg-blur * 10);
            height: calc(100% + $bg-blur * 10);

            background-image: url("/src/assets/images/restaurant_bg.png");
            background-size: cover;
            background-position: calc(50% - var(--translateOffsetX)) calc(50% - var(--translateOffsetY));
            background-repeat: no-repeat;

            filter: blur($bg-blur);
            z-index: -1;
        }

        > h1 {
            text-align: right;
            font-family: "coolvetica crammed";
            color: var(--color-white);
            position: relative;
            font-weight: normal;

            -webkit-user-select: none;
            -moz-user-select: none;
            user-select: none;

            z-index: 1;
            
            $word-spacing: 15px;
            > span {
                font-size: min(12vw, 150px);
                display: inline-block;
                position: relative;
                
                &.space {
                    margin-right: $word-spacing;
                }
                &.accent {
                    color: var(--color-orange);
                }

                &:not(.space) {
                    opacity: 0;
                    transform: translateY(10%);
                }

                --translateY: -3px;
                &::before {
                    content: attr(data-letter);
                    position: absolute;
                    z-index: -1;
                    top: 50%;
                    left: 50%;
                    width: 100%;
                    height: 100%;
                    translate: calc(-50% + var(--translateY)) calc(-50% - var(--translateY));
    
                    color: transparent;
                    -webkit-text-stroke: 2.25px var(--color-orange);
                    font-size: inherit;
                }
            }

        }
        @media screen and (max-width: 600px) {
            padding: 0 7.5px;
            > h1 {
                font-size: 30px;
                > span::before {
                    -webkit-text-stroke-width: .5px;
                }
                > span.space {
                    margin-right: 5px;
                }
            }
        }
    }
</style>