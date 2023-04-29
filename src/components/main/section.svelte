<script lang="ts">
	export let title: string = "";
	export let id: string = "";
</script>

<section {id}>
	{#if title}
		<div class="title">
			<h2 data-title={title}>{title}</h2>
			<hr />
		</div>
	{/if}
	<div class="section-content">
		<slot name="content">
            <p class="no-written">Oups... Ce contenu n'a pas encore été rédigé</p>
        </slot>
	</div>
</section>

<style lang="scss">
	section {
		&:not(:has(> .title)) {
			margin-top: 5vh;
		}

		> .title {
			display: grid;
			align-items: center;
			grid-template-columns: max-content auto;
			gap: 25px;

			padding: 0 15px;

			> h2 {
				$translateSize: 2.5px;
				font-size: min(75px, 20vw);
				font-family: "coolvetica crammed";
				letter-spacing: 2px;
				font-weight: normal;
				position: relative;
				color: transparent;
				margin-left: $translateSize;
				z-index: 1;

				// Ici je ne peux pas utiliser les propriétés background-clip directement sur le text car le ::before a son z-index > background
				&::before {
					content: attr(data-title);
					position: absolute;
					top: 0;
					left: 0;
					translate: -#{$translateSize} $translateSize;

					-webkit-text-stroke: 1.5px var(--color-black);
					z-index: -1;

					transition: 0.15s;
				}

				&::after {
					content: attr(data-title);
					position: absolute;
					top: 0;
					left: 0;

					background: linear-gradient(
						to bottom,
						var(--color-orange),
						var(--color-purple)
					);
					-webkit-background-clip: text;
					-moz-background-clip: text;
					background-clip: text;
					z-index: -1;
				}
			}

			$lineColor: var(--color-orange);
			> hr {
				width: 100%;
				height: 2.5px;
				translate: 0 6px; // Car la font coolvetica (crammed) n'est pas centrée verticalement

				background: linear-gradient(
					to right,
					var(--color-orange) 40%,
					#00000000 0%
				);
				background-size: 15px;
				background-repeat: repeat-x;
				background-position: center;

				overflow: visible;
				margin-right: 20px;
				border: none;

				$ballSize: 15px;
				&::before {
					content: "";
					position: absolute;
					top: 50%;
					left: 0;
					translate: -50% -50%;

					background-color: $lineColor;
					height: $ballSize;
					width: $ballSize;

					border-radius: 9999px;
				}
				&::after {
					content: "";
					position: absolute;
					top: 50%;
					right: 0;
					translate: 50% -50%;

					background-color: $lineColor;
					height: $ballSize;
					width: $ballSize;

					border-radius: 9999px;
				}
			}
		}

		> .section-content {
			padding: 15px 5px;

            > p.no-written {
                text-align: center;
                margin: 15px 0;
                font-family: "coolvetica";
                font-style: italic;
            }
		}

		&:hover > .title > h2::before {
			translate: 0 0;
			text-shadow: 0 0 15px var(--color-black);
		}
	}
</style>
