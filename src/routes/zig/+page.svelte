<script lang="ts">
  import * as combobox from "@zag-js/combobox";
  import { useMachine, normalizeProps } from "@zag-js/svelte";

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
    { translations: { es: "Argentina" }, iso3: "ARG", emoji: "🇦🇷" },
    { translations: { es: "Bolivia" }, iso3: "BOL", emoji: "🇧🇴" },
    { translations: { es: "Colombia" }, iso3: "COL", emoji: "🇨🇴" },
    { translations: { es: "Costa Rica" }, iso3: "CRI", emoji: "🇨🇷" },
    { translations: { es: "Comoras" }, iso3: "COM", emoji: "🇰🇲" },
    { translations: { es: "Congo" }, iso3: "COG", emoji: "🇨🇬" },
    { translations: { es: "Chile" }, iso3: "CHL", emoji: "🇨🇱" },
    { translations: { es: "Ecuador" }, iso3: "ECU", emoji: "🇪🇨" },
    { translations: { es: "México" }, iso3: "MEX", emoji: "🇲🇽" },
    { translations: { es: "Perú" }, iso3: "PER", emoji: "🇵🇪" },
  ];

  let countries: ComboboxData[] = [];

  let options = $state.raw(countries);

  const collection = $derived(
    combobox.collection({
      items: options,
      itemToValue: (item) => item.label,
      itemToString: (item) => item.value,
    })
  );

  const id = $props.id();

  const service = useMachine(combobox.machine, {
    id,
    get collection() {
      return collection;
    },
    onOpenChange() {
      options = countries;
    },
    onInputValueChange({ inputValue }) {
      console.log(inputValue.length, "size");
      if (inputValue.length >= 2) {
        console.log("search");
        const filtered = COUNTRIES_DATA.filter(
          (country) =>
            country.translations?.es &&
            country.translations.es
              .toLowerCase()
              .startsWith(inputValue.toLowerCase())
        );
        console.log("FILTERED:", filtered);
        countries = [
          ...filtered.map(({ translations, iso3, emoji }) => ({
            label: translations.es ?? "",
            value: iso3 ?? "",
            emoji: emoji || "",
          })),
        ];
        let newOptions: ComboboxData[] = [];
        if (filtered && filtered.length > 0) {
          newOptions = countries;
        }
        options = newOptions;
        console.log("Countries updated:", countries);
      } else {
        console.log("clear");
        countries = [];
        options = [];
      }
    },
  });

  const api = $derived(combobox.connect(service, normalizeProps));
</script>

<div {...api.getRootProps()}>
  <label {...api.getLabelProps()}>Select country</label>
  <div {...api.getControlProps()}>
    <input class="bg-gray-400 text-white" {...api.getInputProps()} />
    <button {...api.getTriggerProps()}>▼</button>
  </div>
</div>
<div {...api.getPositionerProps()}>
  {#if options.length > 0}
    <ul {...api.getContentProps()}>
      {#each options as item}
        <li {...api.getItemProps({ item })}>{item.value + item.emoji}</li>
      {/each}
    </ul>
  {/if}
</div>
