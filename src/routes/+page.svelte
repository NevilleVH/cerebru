<script lang="ts">
	import diagrams from '$lib/assets/diagrams.json';
	import FreeChecker from '$lib/components/freechecker.svelte';
	import OptionChecker from '$lib/components/optionchecker.svelte';
	const imageModules = import.meta.glob('$lib/assets/diagrams/*.{png,svg}', {
		eager: true,
		query: {
			//enhanced: true
		}
	});
	//import cerebral_surface_sagittal_lateral from "$lib/assets/cerebral_surface_sagittal_lateral.png"
	const diagramInfo = Object.values(diagrams)
		.sort((a, b) => (a.title > b.title ? 1 : -1))
		.flatMap((info) =>
			imageModules[info.path] ? [{ ...info, img: imageModules[info.path].default }] : []
		);
	let selected = $state(diagramInfo[0]);
	let inputType = $state<'free' | 'options'>('free');
</script>

<div style="width:100vw; padding: 4px;">
	<div style="display: flex; flex-wrap: wrap; gap: 4px; margin-bottom: 4px">
		<div>
			Diagram:
			<select>
				{#each diagramInfo as info}
					<option onclick={() => (selected = info)}>{info.title}</option>
				{/each}
			</select>
		</div>
		<div>
			Input type:
			<select bind:value={inputType}>
				<option value="free">Free</option>
				<option value="options">Options</option>
			</select>
		</div>
	</div>
	{#if selected}
		{#key selected.path}
			<div id="container" style="display: flex;">
				<div id="labels-container">
					<div id="labels" style="overflow-y:scroll; border:solid 1px; padding: 4px">
						{#each selected.labels as label, i}
							<div style="display: flex; justify-content: end; padding: 4px; gap: 4px">
								<div>{i + 1 + '.'}</div>
								{#if inputType === 'free'}
									<FreeChecker {label} />
								{:else}
									<OptionChecker {label} items={selected.labels} />
								{/if}
							</div>
						{/each}
					</div>
				</div>
				<div id="diagram">
					<img style="object-fit: contain;" alt={selected.title} src={selected.img} />
				</div>
			</div>
		{/key}
	{/if}
</div>

<style>
	#labels {
		height: 80vh;
	}

	@media (orientation: portrait) {
		img {
			width: 100%;
		}

		#diagram {
			order: 1;
		}

		#labels-container {
			order: 2;
		}

		#container {
			flex-wrap: wrap;
		}

		#labels {
			height: 50vh;
		}
	}
</style>
