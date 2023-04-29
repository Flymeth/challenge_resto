<script lang="ts">
    import Loader from "./lib/loader.svelte";
    import Header from "./components/header.svelte";
    import Nav from "./components/nav.svelte";
    import Main from "./components/main/main.svelte";
    import Footer from "./components/footer.svelte";    

    let loaded = false;

    const fakeLoadingSeconds = 5//s -> Pour admirer mon beau loader (j'y ai mis du coeur)
    
    let headerAnimation: typeof Header.prototype.animate;
    function then() {
        headerAnimation()
    }

    window.addEventListener('load', () => {
        setTimeout(() => {
            loaded = true
        }, fakeLoadingSeconds * 1000);
    })
</script>

<Loader loading={!loaded} {then}/>
<Nav />
<Header bind:animate={headerAnimation} autoStartOnMount={false} titleAnimation={!navigator.userAgent.includes('Mobile')}/>
<Main />
<Footer/>