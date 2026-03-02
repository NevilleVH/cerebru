<script lang="ts">
    const {label, items}:{label:string, items:string[]} = $props()
    let value = $state("")
    //let distance = $derived(leven(value.toLowerCase(), label.toLowerCase(), {maxDistance: 3}))
</script>

<div style="display: flex;">
    <select bind:value={value}>
        {#each items as item}
            <option value={item}>{item}</option>
        {/each}
    </select>
    <!-- <div>{distance > 2 ? "❌": "✅"}</div> -->
    <div>{label.toLowerCase() !== value.toLowerCase() ? "❌": "✅"}</div>
    <button onclick={() => value = label}>?</button>
</div>