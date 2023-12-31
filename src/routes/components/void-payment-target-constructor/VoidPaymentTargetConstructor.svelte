<script lang="ts">
	import {
		FieldGroup,
		FieldGroupAppendix,
		FieldGroupLabel,
		FieldGroupNumber,
		FieldGroupText,
		ListBox,
		ListBoxButton,
		ListBoxMenu,
		ListBoxOption
	} from '$lib/components';

	import { TRANSPORT } from '$lib/data/transports.data';
	import { join } from '$lib/helpers/join.helper';
	import { constructor } from '$lib/store/constructor.store';
	import { fade, fly } from 'svelte/transition';
</script>

<div class="[ flex flex-col gap-6 ]" in:fly={{ y: 64 }}>
	<div class={join('[ flex flex-col items-stretch gap-2 ]')}>
		<label id="transport-network-label" for="transport-network">Transport Network *</label>
		<div class="[ flex flex-col items-stetch gap-4 ]">
			{#if $constructor.void.transport !== 'other'}
				<div in:fade>
					<ListBox
						value={$constructor.void.transport}
						on:change={(ev) => ($constructor.void.transport = ev.detail)}
					>
						<ListBoxButton>
							<span class="[ inline-block truncate uppercase ]">{$constructor.void.transport}</span>
							<span class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									viewBox="0 0 20 20"
									fill="currentColor"
									aria-hidden="true"
									class="w-5 h-5 text-gray-400"
								>
									<path
										fill-rule="evenodd"
										d="M10 3a1 1 0 01.707.293l3 3a1 1 0 01-1.414 1.414L10 5.414 7.707 7.707a1 1 0 01-1.414-1.414l3-3A1 1 0 0110 3zm-3.707 9.293a1 1 0 011.414 0L10 14.586l2.293-2.293a1 1 0 011.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z"
										clip-rule="evenodd"
									/>
								</svg>
							</span>
						</ListBoxButton>

						<ListBoxMenu items={TRANSPORT.void} let:item>
							<ListBoxOption value={item.value} let:selected>
								<div class={join('[ inline-flex items-center gap-2 transition-all duration-200 ]')}>
									<span class="[ truncate font-medium ]">{item.label}</span>
									<span class="[ truncate font-medium text-gray-400 ]">{item.symbol}</span>
								</div>

								{#if selected}
									<span
										class="[ absolute inset-y-0 left-0 flex items-center pli-3 text-green-500 ]"
									>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											viewBox="0 0 20 20"
											fill="currentColor"
											aria-hidden="true"
											class="w-5 h-5"
										>
											<path
												fill-rule="evenodd"
												d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
												clip-rule="evenodd"
											/>
										</svg>
									</span>
								{/if}
							</ListBoxOption>
						</ListBoxMenu>
					</ListBox>
				</div>
			{/if}

			{#if $constructor.void.transport === 'other'}
				<div class={join('[ flex items-center ]', '[ relative ]')} in:fade>
					<button
						class={join(
							'[ flex items-center justify-between ]',
							'[ absolute inline-start-0 mli-3 p-2 text-gray-50 bg-gray-700 rounded-full outline-none transition-all duration-200 ]',
							'[ focus-within:bg-green-900 focus-within:text-green-50 active:scale-95 ]'
						)}
						type="button"
						title="Back to network menu options"
						on:pointerdown={() => ($constructor.void.transport = 'geo')}
					>
						<svg
							class={join('[ bs-4 is-4 ]')}
							fill="none"
							stroke="currentColor"
							stroke-width="1.5"
							viewBox="0 0 24 24"
							xmlns="http://www.w3.org/2000/svg"
							aria-hidden="true"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								d="M10.5 19.5L3 12m0 0l7.5-7.5M3 12h18"
							/>
						</svg>
					</button>

					<input
						class={join(
							'[ is-full bs-12 plb-2 pis-14 pie-3 text-start bg-gray-900 rounded-md border-none caret-teal-500 ]',
							'[ focus:outline-none focus-visible:ring-4 focus-visible:ring-opacity-75 focus-visible:ring-green-800 focus-visible:ring-offset-green-700 focus-visible:ring-offset-2 ]',
							'[ sm:text-sm ]'
						)}
						type="text"
						id="transport-network"
						placeholder="Other network"
						autocomplete="off"
						aria-labelledby="transport-network-label"
						style="text-transform: uppercase"
						bind:value={$constructor.void.other}
					/>
				</div>
			{/if}
		</div>
	</div>

	<div class={join('[ flex flex-col items-stretch gap-2 ]')}>
		<label id="exchange-point-label" for="exchange-point">Location</label>
		{#if $constructor.void.transport === 'geo'}
			<div class="[ flex gap-4 ]">
				<input
					class={join(
						'[ is-full bs-12 plb-2 pli-3 text-start bg-gray-900 rounded-md border-none caret-teal-500 ]',
						'[ focus:outline-none focus-visible:ring-4 focus-visible:ring-opacity-75 focus-visible:ring-green-800 focus-visible:ring-offset-green-700 focus-visible:ring-offset-2 ]',
						'[ sm:text-sm ]'
					)}
					type="number"
					placeholder="Latitude"
					autocomplete="off"
					aria-labelledby="exchange-point-label"
					bind:value={$constructor.void.params.loc.lang}
				/>
				<input
					class={join(
						'[ is-full bs-12 plb-2 pli-3 text-start bg-gray-900 rounded-md border-none caret-teal-500 ]',
						'[ focus:outline-none focus-visible:ring-4 focus-visible:ring-opacity-75 focus-visible:ring-green-800 focus-visible:ring-offset-green-700 focus-visible:ring-offset-2 ]',
						'[ sm:text-sm ]'
					)}
					type="number"
					id="exchange-point"
					placeholder="Longitude"
					autocomplete="off"
					aria-labelledby="exchange-point-label"
					bind:value={$constructor.void.params.loc.long}
				/>
			</div>
			<small class="[ -mbs-1 text-gray-400 ]">Search for the geocordinates - <a class="[ transition-all duration-200 ] [ visited:text-gray-200 hover:text-gray-300 ]" href="https://gps-coordinates.org/" target="_blank" rel="noreferrer">Latitude & Longitude (DD - Decimal Degrees)</a></small>
		{/if}

		{#if $constructor.void.transport === 'plus'}
			<input
				class={join(
					'[ is-full bs-12 plb-2 pli-3 text-start bg-gray-900 rounded-md border-none caret-teal-500 ]',
					'[ focus:outline-none focus-visible:ring-4 focus-visible:ring-opacity-75 focus-visible:ring-green-800 focus-visible:ring-offset-green-700 focus-visible:ring-offset-2 ]',
					'[ sm:text-sm ]'
				)}
				type="text"
				id="exchange-point"
				placeholder="Plus Code, e.g. 87G8Q2PQ+96"
				autocomplete="off"
				aria-labelledby="exchange-point-label"
				style="text-transform: uppercase"
				bind:value={$constructor.void.params.loc.plus}
			/>
			<small class="[ -mbs-1 text-gray-400 ]">Search for the <a class="[ transition-all duration-200 ] [ visited:text-gray-200 hover:text-gray-300 ]" href="https://plus.codes/map" target="_blank" rel="noreferrer">Plus Code</a></small>
		{/if}

		{#if $constructor.void.transport === 'other'}
			<input
				class={join(
					'[ is-full bs-12 plb-2 pli-3 text-start bg-gray-900 rounded-md border-none caret-teal-500 ]',
					'[ focus:outline-none focus-visible:ring-4 focus-visible:ring-opacity-75 focus-visible:ring-green-800 focus-visible:ring-offset-green-700 focus-visible:ring-offset-2 ]',
					'[ sm:text-sm ]'
				)}
				type="text"
				id="exchange-point"
				placeholder="Point"
				autocomplete="off"
				aria-labelledby="exchange-point-label"
				bind:value={$constructor.void.params.loc.other}
			/>
		{/if}
	</div>

	<FieldGroup>
		<FieldGroupLabel>Beneficiary Full Name</FieldGroupLabel>
		<FieldGroupText
			placeholder="e.g. John Doe"
			bind:value={$constructor.void.params.receiverName.value}
		/>
	</FieldGroup>

	<FieldGroup>
		<FieldGroupLabel>Message for Beneficiary</FieldGroupLabel>
		<FieldGroupText
			placeholder="e.g. ID001"
			bind:value={$constructor.void.params.message.value}
		/>
	</FieldGroup>

	<FieldGroup>
		<FieldGroupLabel>Amount</FieldGroupLabel>
		<FieldGroupNumber placeholder="e.g. 3.14" bind:value={$constructor.void.params.amount.value} />
	</FieldGroup>

	<FieldGroup>
		<FieldGroupLabel>Currency code</FieldGroupLabel>
		<FieldGroupText placeholder="e.g. XCB; USD" bind:value={$constructor.void.params.currency.value} />
		<FieldGroupAppendix>Empty value uses the default network currency.</FieldGroupAppendix>
	</FieldGroup>
</div>
