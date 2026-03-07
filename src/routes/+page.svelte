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
	const diagramInfo = Object.entries(diagrams)
		
		.flatMap(([id, info]) =>
			imageModules[info.path] ? [{ ...info, id, img: imageModules[info.path].default }] : []
		)
		.sort((a, b) => (a.title > b.title ? 1 : -1))
	let selectedId = $state(diagramInfo[0].id)
	let selected = $derived(diagramInfo.find(info => info.id === selectedId));
	let inputType = $state<'free' | 'options'>('free');
	let expandedTerm = $state(-1)
	let dlg = $state<HTMLDialogElement>()
</script>

<div style="padding: 4px;">
	<div style="display: flex; flex-wrap: wrap; gap: 4px; margin-bottom: 4px">
		<div>
			Diagram:
			<select bind:value={selectedId} onchange={() => { expandedTerm = -1}}>
				{#each diagramInfo as info}
					<option value={info.id}>{info.title}</option>
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
	<dialog bind:this={dlg}>
		{#if selected?.labels.at(expandedTerm)}
		{@const {term, description, etymology} = selected.labels[expandedTerm] ?? {}}
			<div>
			<strong>{term}</strong>
			<div>Desciption: {description ?? ""}</div>
			<div>Etymology: {etymology ?? ""}</div>
			<button onclick={() => {
				expandedTerm = -1
				dlg?.close()
			}}>Close</button>
			</div>
		{/if}
	</dialog>
	{#if selected}
		{#key selected.id}
			<div id="container" style="display: flex; width: 90vw">
				<div id="labels-container">
					<div id="labels" style="overflow-y:scroll; border:solid 1px; padding: 4px">
						{#each selected.labels as {term}, i}
						
							<div style="display: flex; justify-content: end; padding: 4px; gap: 4px">
								<div>{i + 1 + '.'}</div>
								{#if inputType === 'free'}
									<FreeChecker label={term} />
								{:else}
									<OptionChecker label={term} items={selected.labels.map(l => l.term)} />
								{/if}
								<button popovertarget="description-dlg" onclick={() => {
									expandedTerm = i
									dlg?.showModal()
								}}>ℹ️</button>
							</div>
						{/each}
					</div>
				</div>
				<div id="diagram" style="width: 100%">
					<img style="object-fit: contain; width: 100%" alt={selected.title} src={selected.img} />
					
				</div>
			</div>
		{/key}
	{/if}
</div>

<style>
	#labels {
		height: 80vh;
	}

	img {
			height: 80vh;
		}



	@media (orientation: portrait) {

		img {
			height: 40vh;
		}

		#diagram {
			order: 1;
		}

		#labels-container {
			order: 2;
			width: 100%;
		}

		#labels {
			width: 100%;
			display: flex;
			flex-direction: column;
			align-items: center;
		}

		#container {
			justify-content: center;
			flex-wrap: wrap;
		}

		#labels {
			height: 50vh;
		}
	}
</style>
