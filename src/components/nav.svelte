<script>
    import logo from "../assets/logo.svg"
    import cuttlery from "../assets/cuttlery.svg"
</script>

<nav>
    {#if document.location.pathname !== "/"}
        <a href="/">
            <img src={logo} alt="TOKIEST's logo" class="logo">
        </a>
    {:else}    
        <a href="#app">
            <img src={logo} alt="TOKIEST's logo" class="logo">
        </a>
    {/if}
    <menu>
        <ul>
            <li><a href="#carte">Notre Carte</a></li>
            <li><a href="#bouteilles">Nos Bouteilles</a></li>
            <li><a href="#restaurant">Notre Restaurant</a></li>
        </ul>
    </menu>
    <div class="order">
        <button>
            <img src={cuttlery} alt="Cuttlery illustration (fork and knife)" class="cuttlery">
            <p>Réserver</p>
        </button>
    </div>
</nav>

<style lang="scss">
    nav {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 65px;

        background-color: rgba(var(--rgb-darkgreen), .75);
        backdrop-filter: blur(15px);

        display: flex;
        align-items: center;
        justify-content: space-between;

        z-index: 999999998;
        font-family: "coolvetica";

        > * {
            width: 100%;
            height: 100%;
        }

        > a {
            width: auto;
            > img {
                height: 100%;
                width: auto;
            }
        }


        > menu {
            margin: 0 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 25px;

            ul {
                list-style: none;
                display: flex;
                align-items: center;
                justify-content: center;
                width: 100%;

                li {
                    margin: 0 15px;
                    a {
                        color: rgba(var(--rgb-white), 1);
                        text-decoration: none;
                        transition: .25s;
                        position: relative;

                        background: linear-gradient(to right, var(--color-orange), var(--color-purple), var(--color-orange), var(--color-purple), var(--color-orange));
                        background-size: 200% 100%;

                        -webkit-background-clip: text;
                        -moz-background-clip: text;
                        background-clip: text;

                        animation: bgAnim 5s infinite linear;

                        &:hover {
                            color: rgba(var(--rgb-white), 0);
                            text-shadow: 0 0 5px rgba(var(--rgb-orange), .15);
                        }

                        @keyframes bgAnim {
                            from {
                                background-position-x: 0;
                            }
                            to {
                                background-position-x: 100%;
                            }
                        }
                    }
                }
            }

            @media screen and (max-width: 770px) {
                font-size: 15px;
            }
            @media screen and (max-width: 580px) {
                font-size: 12.5px;
                > ul > li {
                    margin: 5px;
                }
            }
            @media screen and (max-width: 475px) {
                display: none;
            }
        }

        > .order {
            $W: 65px;
            $paddingLR: 10px;
            $extended-W: $W + 15px;
            $animationDuration: .15s;

            height: 100%;
            width: $extended-W + $paddingLR * 2;
            position: relative;

            > button {
                background-color: var(--color-darkgreen);
                border: none;
                outline: none;

                position: absolute;
                top: 0;
                right: 0;
                height: 100%;
                width: $W;
                padding: 0 $paddingLR 0 $paddingLR + 5px;
                border-radius: 15px 0 0 15px;
    
                cursor: pointer;
    
                display: flex;
                align-items: center;
                flex-direction: column;
                justify-content: center;

                color: var(--color-white);
                font-family: inherit;
                font-size: 10px;
    
                transition: $animationDuration ease-in-out;

                > p {
                    height: 0;
                    opacity: 0;
                    text-align: center;
                    margin-top: 0;
                    transition: $animationDuration ease-in-out;
                }

                > img {
                    height: 50%;
                    width: auto;
                }

                &:hover {
                    width: $extended-W;
                    padding-bottom: calc($paddingLR / 2);
                    > p {
                        height: 10px;
                        opacity: 1;
                        margin-top: 5px;
                    }
                }
            }
        }
    }
</style>