<script>
    import { createEventDispatcher } from 'svelte';
 	import Editor from '../_Editor/index.svelte';
    const dispatch = createEventDispatcher();

    export let name;
    export let code;
    export let index;
    export let main;
    export let isEntry;
    export let codeSplitting;

    function selectName(input) {
      input.setSelectionRange(0, input.value.length - 3);
    }
</script>

<style>
	.module {
		margin: 0 0 1em 0;
		border: 1px solid #f4f4f4;
	}

	header {
		width: 100%;
		border-bottom: 1px solid #f4f4f4;
	}

	.entry-module {
		border: 1px solid #ccc;
	}

	.entry-module-name {
		display: block;
		padding: 0.5em;
	}

	.entry-module-label {
		color: #555;
		opacity: 0.6;
		position: absolute;
		right: 0;
		padding-right: 0.5em;
	}

	.code-splitting input {
		padding: 0.8em;
	}

	button {
		position: absolute;
		display: block;
		right: 0;
		font-family: inherit;
		font-size: inherit;
		padding: 0.5em;
		margin: 0;
		background-color: transparent;
		border: none;
		cursor: pointer;
		outline: none;
		opacity: 0.4;
		-webkit-transition: opacity 0.2s;
		transition: opacity 0.2s;
	}

	.code-splitting button {
		padding: 0.2em;
	}

	.toggle-entry {
		bottom: 0;
		color: #555;
	}

	.remove {
		top: 0;
		color: #e94c43;
	}

	button:hover,
	button:active,
	.entry-module .toggle-entry:hover,
	.entry-module .toggle-entry:active {
		opacity: 1;
		background-color: transparent;
	}

	button .label {
		position: absolute;
		right: 100%;
		opacity: 0;
		-webkit-transition: opacity 0.2s;
		transition: opacity 0.2s;
	}

	.entry-module .toggle-entry .label {
		opacity: 1;
	}

	.entry-module button.toggle-entry {
		opacity: 0.6;
	}

	button:hover .label, button:active .label {
		opacity: 0.6;
	}

	.icon-cancel, .icon-plus, .icon-minus {
		font-size: 0.8em;
	}
</style>

<article class='module {main || isEntry ? "entry-module" : ""} {codeSplitting ? "code-splitting" : ""}'>
	<header>
		{#if main}
			<span class='entry-module-name'>main.js<span class='entry-module-label'>(entry module)</span></span>
		{:else}
			<input class='module-name' bind:value='{name}' on:focus='{() => selectName(this)}' placeholder='foo.js'>

		    <button class='remove' on:click='{() => dispatch("remove")}'>
				<span class='label'>remove</span>
				<span class='icon-cancel'></span>
			</button>
			{#if codeSplitting}
				<button class='toggle-entry' on:click='{() => dispatch("toggle-entry")}'>
					<span class='label'>(entry&nbsp;module)</span>
					{#if isEntry}
						<span class='icon-minus'></span>
					{:else}
						<span class='icon-plus'></span>
					{/if}
				</button>
			{/if}
		{/if}
	</header>
	<Editor bind:code/>
</article>
