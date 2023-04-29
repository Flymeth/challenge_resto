<script lang="ts">
    import logo from "../assets/logo.svg"
    import Transitor from "../lib/transitor.svelte";
</script>

<footer>
    <Transitor toColor="var(--background-color)" height={30} reverse={"x"}/>

    <img src={logo} alt="TOKIEST's logo">
    <div class="infos">
        <p>Restaurant factice réalisé pour un concours sur le serveur discord <a href="https://discord.com/channels/655077317911117860/1041772720066674760/1097442201799237662" target="_blank">du repaire du web</a>.</p>
        <p>Réalisé et conçu par <a href="https://flymeth.net" target="_blank">Flymeth</a>.</p>
        <p>Iconne inspiré du design de <a href="https://fr.freepik.com/vecteurs-libre/restaurant-black-icones_1010199.htm#query=restaurant&position=0&from_view=search&track=robertav1_2_sidr" target="_blank">Macrovector</a></p>
        <p><a href="https://www.figma.com/file/gthbaQLw8Ma0VxbWneVpUq/Resto-design?node-id=11%3A7&t=Q6S4Q95LwbrC51RG-1" target="_blank">Mackette Figma</a> disponible publiquement.</p>
    </div>
</footer>

<style lang="scss">
    footer {
        display: grid;
        grid-template-columns: repeat(2, 50%);
        align-items: center;
        position: relative;
        --background-color: var(--color-darkgreen);

        background-color: var(--background-color);

        img {
            height: 100%;
        }
        > .infos {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100%;
            color: var(--color-white);
            padding: 15px;

            text-align: center;
            opacity: .5;

            font-family: "coolvetica";
            font-size: 12.5px;

            a {
                color: inherit;
                &:hover {
                    text-decoration: underline double;
                }
            }
        }

        @media screen and (max-width: 600px) {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;

            img {
                width: 50%;
            }
        }
    }
</style>