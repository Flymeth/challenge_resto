<script>
    import Menu from "../../lib/menu/menu.svelte";
    import Vins from "../../lib/vins.svelte";
    import Infos from "../../lib/infos.svelte";
    import Section from "./section.svelte";
    import Transition from "../../lib/transition.svelte";
</script>

<main>
    <Transition toColor= "var(--background-color)"/>

    <article class="content" style="font-size: 25px;">
        <Section title="Notre Carte" id="carte">
            <Menu slot="content"/>
        </Section>
        <Section title="Nos Bouteilles" id="bouteilles">
            <Vins slot="content"/>
        </Section>
        <Section id="restaurant">
            <Infos slot="content"/>
        </Section>
    </article>
</main>

<style lang="scss">
    main {
        // Utilisé dans la transition svg
        --background-color: var(--color-white);

        background-color: var(--background-color);
        position: relative;
        min-height: 50vh;

        > .content {
            padding: 5px 10px;
        }
    }
</style>