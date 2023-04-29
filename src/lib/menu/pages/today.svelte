<script lang="ts">
    import {today} from "../../../database/menu.json"

    export let currency = "EUR"
    export let locale = "fr-FR"

    const titles: {[key in keyof typeof today]: string} = {
        entry: "L'entrée du jour",
        dish: "Le plat du jour",
        dessert: "Le dessert du jour"
    }
</script>

<div class="today">
    <div class="title">
        <h3 data-horaires="De 11h30 à 22h00">Aujourd'hui</h3>
    </div>
    <article>
        {#each Object.keys(today) as type}
            {@const {name, price} = today[type]}
            <section>
                <h4>{titles[type]}</h4>
                <div class="content">
                    <p class="name">{name}</p>
                    <span class="line"></span>
                    <p class="price">{price.toLocaleString(locale, {style: "currency", currency, minimumFractionDigits: 2})}</p>
                </div>
            </section>
        {/each}
    </article>
</div>

<style lang="scss">
    @import "../styles.scss";

    .today {
        height: 100%;
        display: grid;
        grid-template-rows: max-content auto;
        grid-template-columns: 100%;
        justify-content: center;

        > .title {
            display: flex;
            align-items: center;
            justify-content: center;

            > h3 {
                text-align: center;
                color: var(--color-purple);
                font-family: "coolvetica compressed";
                font-size: 50px;
                text-decoration: underline;
                text-decoration-thickness: 2px;
                position: relative;
                display: inline;
    
                &::after {
                    content: attr(data-horaires);
                    color: var(--color-black);
                    position: absolute;
                    bottom: 0;
                    right: 0;
                    translate: 0 50%;
                    font-size: .25em;
                    font-family: "coolvetica condensed";
                    font-weight: normal;
                }
            }
        }
        
        article {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-evenly;
            text-transform: capitalize;

            > section {
                width: min(300px, 100%);
                > h4 {
                    font-size: 40px;
                    font-family: "coolvetica crammed";
                    text-decoration: underline;
                    text-decoration-thickness: 2px;
                    font-weight: normal;
                    color: var(--color-orange);
                    text-shadow: 0 0 15px rgba(var(--rgb-black), .25);
                    text-align: center;
                }

                > .content {
                    position: relative;
                    display: inline-grid;
                    align-items: center;
                    grid-template-columns: max-content auto max-content;
                    width: 100%;
                    gap: 10px;
                    font-size: 16px;
                    
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
</style>