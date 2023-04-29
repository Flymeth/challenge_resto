<script context="module">
    let transition_id = 0
</script>

<script lang="ts">
    export let toColor = "black";
    export let height = 50;
    export let reverse: "x" | "y" | "xy" | undefined = undefined;

    const thisTransitionID= transition_id
    transition_id++
</script>

<div class="transition" id="transition_{thisTransitionID}" style={reverse && `scale: ${reverse.includes("x") ? -1 : 1} ${reverse.includes("y") ? -1 : 1};`}>
    <svg width="100%" viewBox="0 0 1436 {height}" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M0 0V{height}H1436L0 0Z" fill="url(#transition_gradiant_{thisTransitionID})"/>
        <defs>
            <linearGradient id="transition_gradiant_{thisTransitionID}" x1="718" y1="0" x2="718" y2="{height}" gradientUnits="userSpaceOnUse">
                <stop stop-color="{toColor}"/>
                <stop offset="0" stop-color="{toColor}" stop-opacity="0"/>
                <stop offset="1" stop-color="{toColor}"/>
            </linearGradient>
        </defs>
    </svg>
</div>

<style>
    .transition {
        position: absolute;
        bottom: 100%;
        left: 0;
        width: 100%;
        display: flex;
        align-items: flex-end;
        justify-content: center;
    }
</style>