<script lang="ts">
	import { Dashboard, DashboardModal, DragDrop, ProgressBar } from '@uppy/svelte';
	import Uppy from '@uppy/core';
	import Webcam from '@uppy/webcam';
	import { browser } from '$app/environment';
	import AwsS3Multipart from '@uppy/aws-s3-multipart';
	import Transloadit from '@uppy/transloadit';

	let uppy1: any;
	let uppy2: any;

	const createUppy = () => {
		uppy1 = new Uppy().use(Webcam).use(Transloadit, {
			assemblyOptions: {
				params: {
					auth: { key: 'ca1fb7f141444e9095d2edc66b21e71c' },
					template_id: 'f6e03ed35b6e4b98999926034c6f982d'
				}
			}
		});
		uppy2 = new Uppy().use(Webcam).use(Transloadit, {
			assemblyOptions: {
				params: {
					auth: { key: 'ca1fb7f141444e9095d2edc66b21e71c' },
					template_id: 'f6e03ed35b6e4b98999926034c6f982d'
				}
			}
		});
	};

	$: browser && createUppy();
	$: open = false;
	$: showInlineDashboard = true;
</script>

{#if browser}
	<main>
		<h1>Welcome to the <code>@uppy/svelte</code> demo!</h1>
		<h2>Inline Dashboard</h2>
		<label>
			<input type="checkbox" bind:checked={showInlineDashboard} />
			Show Dashboard
		</label>
		{#if showInlineDashboard}
			<Dashboard uppy={uppy1} plugins={['Webcam']} />
		{/if}
		<h2>Modal Dashboard</h2>
		<div>
			<button on:click={() => (open = true)}>Show Dashboard</button>
			<DashboardModal
				uppy={uppy2}
				{open}
				props={{
					onRequestCloseModal: () => (open = false),
					plugins: ['Webcam']
				}}
			/>
		</div>

		<h2>Drag Drop Area</h2>
		<DragDrop uppy={uppy1} />

		<h2>Progress Bar</h2>
		<ProgressBar
			uppy={uppy1}
			props={{
				hideAfterFinish: false
			}}
		/>
	</main>
{/if}

<style global>
	@import '@uppy/core/dist/style.min.css';
	@import '@uppy/dashboard/dist/style.min.css';
	@import '@uppy/drag-drop/dist/style.min.css';
	@import '@uppy/progress-bar/dist/style.min.css';
	input[type='checkbox'] {
		user-select: none;
	}
</style>
