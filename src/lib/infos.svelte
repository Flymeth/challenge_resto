<script lang="ts">
    import icon from "../assets/icon.svg"
    import { onMount } from "svelte";
    import gsap, {Power2} from "gsap";

    export let imageAnimation= !navigator.userAgent.includes("Mobile");
    export let maxSkew = 3;
    let infosIcon: HTMLImageElement;

    function startHandleAnimation() {
        window.addEventListener('mousemove', (event) => {
            const {x, y} = event

            const rect = infosIcon.getBoundingClientRect()
            const iconCenteredPosOnScreen = [
                rect.left + infosIcon.clientWidth/2,
                rect.top + infosIcon.clientHeight/2
            ]

            const offsetX = x - iconCenteredPosOnScreen[0]
            const offsetY = y - iconCenteredPosOnScreen[1]

            const signX = Math.max(-1, Math.min(1, offsetY/iconCenteredPosOnScreen[1]))
            const signY = Math.max(-1, Math.min(1, offsetX/iconCenteredPosOnScreen[0]))
            
            const skew = [
                Math.max(-maxSkew, Math.min(maxSkew, offsetX / 10 * signX)),
                Math.max(-maxSkew, Math.min(maxSkew, offsetY / 10 * signY))
            ]

            gsap.to(infosIcon, {
                skewX: skew[0],
                skewY: skew[1],
                ease: Power2.easeOut,
                duration: 2
            })
        })
    }

    onMount(() => {
        if(imageAnimation) startHandleAnimation()
    })
</script>

<div class="infos">
    <img src={icon} alt="TOKIEST's Icon" bind:this={infosIcon}>
    <div class="content">
        <p>
            TOKIEST est un restaurant <span class="important">3 fois étoilés</span> dans le centre de Lyon.
        </p>
        <p>
            Vous pourrez y découvrir des <span class="important">locaux flamboyants</span> conçuent pour votre confort et des mêts dignes de la <span class="important">gastronomie à la française</span>.
        </p>
        <p>
            Fondé en 1996 par le chef Duthy, c’est aujourd’hui un lieu de partage entre familles, amis, collègues, ...
        </p>
        <p>
            Grâce à notre parking mis à votre disposition, vous n'aurez aucun mal à venir passer un moment de convivialité et de détente
            dans notre réstaurant <span class="important">ouvert du Lundi au Samedi, de 9h30 à 23h00</span>
            , situé au <span class="important"><a href="https://goo.gl/maps/QXEmhfyn3J6Gve9m9" target="_blank">47 Rue Duquesne, Lyon 6</a></span>.
        </p>
        <br />
        <p>
            Vous souhaitez réserver ou obtenir une information?
        </p>
        <p>
            Rien de plus simple: contactez nous au <span class="important"><a href="tel:+33123456789">01.23.45.67.89</a></span> ou par email à l'adresse <span class="important"><a href="mailto:restaurant@tokiest.fr">restaurant@tokiest.fr</a></span>
        </p>
    </div>
</div>

<style lang="scss">
    .infos {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        > img {
            width: min(125px, 25%);
            margin: 0 auto;
            scale: 1;

            transition: scale .15s;
            &:hover {
                scale: 1.1 !important;
            }
        }
        > .content {
            padding: 10px;
            font-family: "coolvetica condensed";
            text-align: center;
            max-width: 950px;

            @media screen and (max-width: 650px) {
                font-size: 20px;
            }

            > p {
                margin: 5px 0;
            }

            span.important {
                color: var(--color-green);
                position: relative;
                z-index: 1;

                display: inline-block;

                &:not(:has(a)) {
                    -webkit-user-select: all;
                    -moz-user-select: all;
                    user-select: all;
                }
                &::before {
                    content: "";
                    position: absolute;
                    top: 0;
                    left: 50%;
                    width: calc(100% + 2.5px);
                    height: 75%;
                    translate: -50% 25%;
                    z-index: -1;

                    background-color: rgba(var(--rgb-orange), .2);
                    background: linear-gradient(to top, rgba(var(--rgb-orange), .2), rgba(var(--rgb-purple), .2));
                }
            }
        }

        a {
            color: inherit;
            margin: 0 5px;
            font-style: italic;
            text-decoration-thickness: .05em !important;
            text-decoration: underline wavy;
            &:hover {
                text-decoration: underline;
            }
        }
    }
</style>