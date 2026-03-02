<script lang="ts">
	import diagrams from '$lib/assets/diagrams.json';
	import FreeChecker from '$lib/components/freechecker.svelte';
    import OptionChecker from '$lib/components/optionchecker.svelte';
    import {base} from "$app/paths"
	// const imageModules = import.meta.glob('$lib/assets/diagrams/*.{png,svg}', {
	// 	eager: true,
	// 	query: {
	// 		//enhanced: true
	// 	}
	// });
	//import cerebral_surface_sagittal_lateral from "$lib/assets/cerebral_surface_sagittal_lateral.png"
	const diagramInfo = Object.values(diagrams).sort((a, b) => (a.title > b.title ? 1 : -1));
	// .flatMap((info) =>
	// 	imageModules[info.path] ? [{ ...info, img: imageModules[info.path].default }] : []
	// );
	let selected = $state(diagramInfo[0]);
    let inputType = $state<"free" | "options">("free")
</script>

<div style="padding: 4px;">
    Diagram:
	<select>
		{#each diagramInfo as info}
			<option onclick={() => (selected = info)}>{info.title}</option>
		{/each}
	</select>
    Input type:
    <select bind:value={inputType}>
        <option value="free">Free</option>
        <option value="options">Options</option>
    </select>
</div>
{#if selected}
	<div style="display: flex">
		<div>
			<div style="height: 90vh; overflow-y:scroll; border:solid 1px">
				{#each selected.labels as label, i}
					<div style="display: flex; justify-content: end; padding: 4px; gap: 4px">
						<div>{i + 1 + '.'}</div>
                        {#if inputType === "free"}
                             <FreeChecker {label}/>
                        {:else}
                            <OptionChecker label={label} items={selected.labels}/>
                        {/if}
                        
						
					</div>
				{/each}
			</div>
		</div>
		<div><img alt={selected.title} src={base + selected.path} /></div>
	</div>
{/if}
