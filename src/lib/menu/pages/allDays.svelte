<script lang="ts">
    import {allDays, menus} from "../../../database/menu.json"
    import {marked} from "marked"

    export let currency = "EUR"
    export let locale = "fr-FR"

    const titles: {[key in keyof typeof allDays]: string} = {
        entries: "Nos Entrées",
        dishes: "Nos Plats",
        desserts: "Nos Desserts",
        softdrinks: "Nos Boissons Softs",
        harddrinks: "Nos Boissons Alcoolisées",
        hotdrinks: "Nos Boissons Chaudes"
    }
</script>

<article>
    {#each Object.keys(allDays) as type, index}
        <section id={type} style="grid-area: s{index + 1};">
            <h3>{titles[type]}</h3>
            <ul>
                {#each allDays[type] as item}
                    <li data-dish={item.name}>
                        <div class="container">
                            <!-- Ici il y a besoin d'un container pour garder les ::marker des li et avoir un display flex en même temps -->
                            <p class="name">{item.name}</p>
                            <span class="line"></span>
                            <p class="price">{item.price.toLocaleString(locale, {style: "currency", currency, minimumFractionDigits: 2})}</p>
                        </div>
                    </li>
                {/each}
            </ul>
        </section>
    {/each}
    <section id="menus" style="grid-area: menus;">
        <h3>Menus</h3>
        <ul class="menus">
            {#each menus as menu}
                <li>
                    <div class="menu-infos">
                        <h4 class="menu-name">{menu.name}</h4>
                        <span class="line"></span>
                        <span class="menu-price">{menu.price.toLocaleString(locale, {style: "currency", currency, minimumFractionDigits: 2})}</span>
                    </div>
                    <ol class="menu-content">
                        {#each menu.includes as item}
                            <li>{@html marked.parse(item)}</li>
                        {/each}
                    </ol>
                </li>
            {/each}
        </ul>
    </section>
</article>
<style lang="scss">
    @import "../styles.scss";

    article {
        display: grid;
        width: 100%;
        height: 100%;
        overflow: hidden;
        gap: 7.5px;
        position: relative;
        grid-template-areas: 
            "s1 s4"
            "s2 s5"
            "s3 s6"
            "menus menus"
        ;
        grid-template-columns: repeat(2, 50%);
        text-transform: capitalize;

        

        > section {
            font-size: 32px;
            @media screen and (max-width: 850px) {
                font-size: 25px;
            }
            @media screen and (max-width: 380px) {
                font-size: 20px;
            }

            > h3 {
                font-family: "coolvetica crammed";
                text-decoration: underline;
                text-decoration-thickness: 2px;
                font-weight: normal;
                color: var(--color-orange);
                text-shadow: 0 0 15px rgba(var(--rgb-black), .25);
            }

            width: 100%;

            &:not(#menus) {
                > ul {
                    list-style: disc inside;
                    $substract-marker-margin: 10px;
                    > li {
                        font-size: .5em;
                        width: 100%;
                        > .container {
                            position: relative;
                            left: -#{$substract-marker-margin};
                            display: inline-grid;
                            align-items: center;
                            grid-template-columns: max-content auto max-content;
                            width: calc(100% - (32.5px - $substract-marker-margin)); // Pour ne pas prendre en compte le margin-right du ::marker
                            gap: 5px;

                            > .name {
                                font-family: "coolvetica condensed";
                                font-weight: normal;
                            }
                            > .line {
                                @include dashedLine(1px);
                            }
                            > .price {
                                font-family: "coolvetica compressed";
                                font-size: .75em;
                                color: var(--color-purple);
                            }
                        }
                    }
                }
            }
            &#menus {
                > ul {
                    list-style: none;
                    display: grid;
                    grid-template-columns: repeat(3, auto);
                    width: calc(100% - 5px); // Car le symbole de la monnaie (€) n'est pas prit en compte

                    gap: 10px;
                    padding: 0 5px;
                    > li {
                        display: grid;
                        grid-template-rows: max-content auto;
                        > .menu-infos {
                            position: relative;
                            display: inline-grid;
                            align-items: center;
                            grid-template-columns: max-content auto max-content;
                            font-size: .75em;
                            width: 100%;
                            gap: 5px;

                            > .menu-name {
                                font-family: "coolvetica condensed";
                                font-weight: normal;
                            }
                            > .line {
                                @include dashedLine(1px);
                            }
                            > .menu-price {
                                font-family: "coolvetica compressed";
                                font-size: .75em;
                                color: var(--color-purple);
                            }
                        }
                        > .menu-content {
                            list-style: none;
                            display: flex;
                            flex-direction: column;
                            align-self: center;
                            justify-content: center;
                            font-family: "coolvetica";
                            font-size: .4em;
                            text-align: center;
                            > li:not(:last-of-type)::after {
                                content: "+";
                            }
                        }
                    }
                }
            }
        }
    }
</style>