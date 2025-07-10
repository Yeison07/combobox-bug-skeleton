<script lang="ts">
	import { Combobox } from '@skeletonlabs/skeleton-svelte';

	interface ComboboxData {
		label: string;
		value: string;
		emoji: string;
	}

	interface CountryResponse {
		translations: { es: string };
		iso3: string;
		emoji?: string;
	}

	const COUNTRIES_DATA: CountryResponse[] = [
		{ translations: { es: 'Argentina' }, iso3: 'ARG', emoji: '🇦🇷' },
		{ translations: { es: 'Bolivia' }, iso3: 'BOL', emoji: '🇧🇴' },
		{ translations: { es: 'Colombia' }, iso3: 'COL', emoji: '🇨🇴' },
		{ translations: { es: 'Costa Rica' }, iso3: 'CRI', emoji: '🇨🇷' },
		{ translations: { es: 'Comoras' }, iso3: 'COM', emoji: '🇰🇲' },
		{ translations: { es: 'Congo' }, iso3: 'COG', emoji: '🇨🇬' },
		{ translations: { es: 'Chile' }, iso3: 'CHL', emoji: '🇨🇱' },
		{ translations: { es: 'Ecuador' }, iso3: 'ECU', emoji: '🇪🇨' },
		{ translations: { es: 'México' }, iso3: 'MEX', emoji: '🇲🇽' },
		{ translations: { es: 'Perú' }, iso3: 'PER', emoji: '🇵🇪' }
	];

	let countries: ComboboxData[] = $state([]);
	let selectedCountry = $state(['']);

	async function searchCountries(input: string): Promise<void> {
		try {
			console.log(input.length, 'size');
			if (input.length >= 2) {
				console.log('search');
				const filtered = COUNTRIES_DATA.filter(
					(country) =>
						country.translations?.es &&
						country.translations.es.toLowerCase().startsWith(input.toLowerCase())
				);
				console.log('FILTERED:', filtered);
				countries = [
					...filtered.map(({ translations, iso3, emoji }) => ({
						label: translations.es ?? '',
						value: iso3 ?? '',
						emoji: emoji || ''
					}))
				];
				console.log('Countries updated:', countries);
			} else {
				console.log('clear');
				countries = [];
			}
		} catch (e) {
			console.error('Error fetching countries:', e);
			countries = [];
		}
	}
</script>

<div style="margin-bottom: 24px;">
	<h2
		style="border-bottom: 1px solid #ccc; padding-bottom: 8px; font-size: 1.25rem; font-weight: bold; color: #333; display: flex; align-items: center; gap: 8px;"
	>
		Location
	</h2>

	<div style="display: grid; grid-template-columns: 1fr; gap: 24px; max-width: 800px;">
		<div style="margin-bottom: 16px;">
			<label
				style="display: flex; align-items: center; gap: 8px; font-size: 0.95rem; font-weight: 500; color: #444;"
				for="country"
			>
				Country
			</label>
			<Combobox
				data={countries}
				value={selectedCountry}
				onValueChange={(e) => (selectedCountry = e.value)}
				onInputValueChange={(e) => searchCountries(e.inputValue)}
				label=""
				placeholder="Select..."
			>
				{#snippet item(item)}
					<div style="display: flex; justify-content: space-between; width: 100%; gap: 8px;">
						<span>{item.label}</span>
						<span>{item.emoji}</span>
					</div>
				{/snippet}
			</Combobox>

			<select bind:value={selectedCountry} style="width: 100%; padding: 6px; margin-top: 8px;">
				{#each countries as country}
					<option value={country.value}>{country.label} {country.emoji}</option>
				{/each}
			</select>
		</div>
	</div>
</div>

