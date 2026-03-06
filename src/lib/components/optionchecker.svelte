<script lang="ts">
    const {label, items}:{label:string, items:string[]} = $props()
    let value = $state("")
    const sortedItems = $derived(items.toSorted())
    //let distance = $derived(leven(value.toLowerCase(), label.toLowerCase(), {maxDistance: 3}))
</script>

<div style="display: flex; gap: 2px">
    <select bind:value={value}>
        {#each sortedItems as item}
            <option value={item}>{item}</option>
        {/each}
    </select>
    <!-- <div>{distance > 2 ? "❌": "✅"}</div> -->
    <div>{label.toLowerCase().trim() !== value.toLowerCase().trim() ? "❌": "✅"}</div>
    <button onclick={() => value = label}>?</button>
</div>