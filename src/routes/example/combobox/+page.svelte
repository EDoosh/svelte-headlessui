<script lang="ts">
    import { createCombobox, Transition, type Item } from "svelte-headlessui";
    import Selector from "$icons/Selector.svelte";
    import Check from "$icons/Check.svelte";

    // prettier-ignore
    const people = [
        {key: 1, name: 'Wade Cooper'},
        {key: 2, name: 'Arlene Mccoy'},
        {key: 3, name: 'Devon Webb'},
        {key: 4, name: 'Tom Cook'},
        {key: 5, name: 'Tanya Fox'},
        {key: 6, name: 'Hellen Schmidt'},
    ];

    const combobox = createCombobox({
        label: "People",
        selected: [people[2]!],
        onselect(value) {
            console.log("select", { ...value[0] });
        },
        getInputValue(val) {
            return val.name;
        }
    });

    const filtered = $derived(
        people.filter((person) =>
            person.name
                .toLowerCase()
                .replace(/\s+/g, "")
                .includes(combobox.filter.toLowerCase().replace(/\s+/g, ""))
        )
    );
</script>

<div class="fixed top-16 w-72">
    <div class="relative mt-1">
        <div
            class="relative w-full cursor-default overflow-hidden rounded-lg bg-white text-left shadow-md focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75 focus-visible:ring-offset-2 focus-visible:ring-offset-teal-300 text-sm"
        >
            <input
                use:combobox.input
                class="w-full border-none py-2 pl-3 pr-10 leading-5 text-gray-900 focus:ring-0 text-sm"
                value={combobox.selected[0]}
            />
            <button
                use:combobox.button
                class="absolute inset-y-0 right-0 flex items-center pr-2"
                type="button"
            >
                <Selector class="h-5 w-5 text-gray-400" />
            </button>
        </div>

        <Transition
            show={combobox.expanded}
            leave="transition ease-in duration-100"
            leaveFrom="opacity-100 duration-100"
            leaveTo="opacity-0 duration-100"
            onafterleave={() => combobox.reset()}
        >
            <ul
                use:combobox.items
                class="absolute mt-1 max-h-60 w-full overflow-auto rounded-md bg-white py-1 text-sm shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
            >
                {#each filtered as value (value.key)}
                    {@const active = combobox.isActive(value)}
                    {@const selected = combobox.isSelected(value)}
                    <li
                        class="relative cursor-default select-none py-2 pl-10 pr-4 {active
                            ? 'bg-teal-600 text-white'
                            : 'text-gray-900'}"
                        use:combobox.item={{ value }}
                    >
                        <span
                            class="block truncate {selected
                                ? 'font-medium'
                                : 'font-normal'}">{value.name}</span
                        >
                        {#if selected}
                            <span
                                class="absolute inset-y-0 left-0 flex items-center pl-3 {active
                                    ? 'text-white'
                                    : 'text-teal-600'}"
                            >
                                <Check class="h-5 w-5" />
                            </span>
                        {/if}
                    </li>
                {:else}
                    <li
                        class="relative cursor-default select-none py-2 pl-10 pr-4 text-gray-900"
                    >
                        <span class="block truncate font-normal"
                            >Nothing found</span
                        >
                    </li>
                {/each}
            </ul>
        </Transition>
    </div>
</div>
