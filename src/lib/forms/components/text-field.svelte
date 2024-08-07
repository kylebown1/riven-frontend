<script lang="ts" context="module">
	// eslint-disable-next-line @typescript-eslint/no-unused-vars
	import type { FormPath } from 'sveltekit-superforms';

	type T = Record<string, unknown>;
	type U = unknown;
</script>

<script lang="ts" generics="T extends Record<string, unknown>, U extends FormPath<T>">
	import { type ControlProps, type FieldProps } from 'formsnap';
	import clsx from 'clsx';
	import type { Writable } from 'svelte/store';
	import { Input } from '$lib/components/ui/input';
	import type { SuperForm } from 'sveltekit-superforms';
	import { formatWords } from '$lib/helpers';
	import * as Form from '$lib/components/ui/form';

	// eslint-disable-next-line @typescript-eslint/no-unused-vars
	type $$Props = FieldProps<T, U> &
		ControlProps & {
			label?: string;
			fieldDescription?: string;
			// eslint-disable-next-line @typescript-eslint/no-explicit-any
			formData: Writable<any>;
			isProtected?: boolean;
		};

	export let form: SuperForm<T>;
	export let name: U;
	export let label: string = formatWords(name as string);
	export let fieldDescription: string | undefined = undefined;
	// eslint-disable-next-line @typescript-eslint/no-explicit-any
	export let formData: Writable<any>;
	export let isProtected: boolean = false;
</script>

<Form.Field {form} {name}>
	<Form.Control let:attrs {...$$restProps}>
		<div
			class={clsx('flex max-w-6xl flex-col items-start gap-2 md:flex-row md:gap-4', {
				'md:items-center': !fieldDescription
			})}
		>
			<div class="flex w-full min-w-48 flex-col items-start gap-2 md:w-48">
				<Form.Label>{label}</Form.Label>
				{#if fieldDescription}
					<p class="text-xs text-muted-foreground">{fieldDescription}</p>
				{/if}
			</div>
			{#if isProtected}
				<Input
					{...attrs}
					class={clsx('transition-all duration-300', {
						'blur-sm hover:blur-none focus:blur-none': $formData[name].length > 0
					})}
					type="text"
					spellcheck="false"
					autocomplete="false"
					bind:value={$formData[name]}
				/>
			{:else}
				<Input
					{...attrs}
					type="text"
					spellcheck="false"
					autocomplete="false"
					bind:value={$formData[name]}
				/>
			{/if}
		</div>
	</Form.Control>

	<Form.FieldErrors class="mt-2 text-xs text-red-500" />
</Form.Field>
