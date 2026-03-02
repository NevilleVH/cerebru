<script lang="ts">
	//import leven from "leven";
    const {label}:{label:string} = $props()
    let value = $state("")
    //let distance = $derived(leven(value.toLowerCase(), label.toLowerCase(), {maxDistance: 3}))
</script>

<div style="display: flex;">
    <input bind:value={value}>
    <!-- <div>{distance > 2 ? "❌": "✅"}</div> -->
    <div>{label.toLowerCase() !== value.toLowerCase() ? "❌": "✅"}</div>
    <button onclick={() => value = label}>?</button>
</div>