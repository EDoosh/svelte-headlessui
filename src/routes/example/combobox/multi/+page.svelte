<script lang="ts">
    import { Transition, createCombobox } from "svelte-headlessui";
    import Selector from "$icons/Selector.svelte";
    import Check from "$icons/Check.svelte";
    import Deselect from "$icons/Deselect.svelte";

    // prettier-ignore
    const people = [
		{ key: 1, name: 'Wade Cooper' },
		{ key: 2, name: 'Arlene Mccoy' },
		{ key: 3, name: 'Devon Webb' },
		{ key: 4, name: 'Tom Cook' },
		{ key: 5, name: 'Tanya Fox' },
		{ key: 6, name: 'Hellen Schmidt' },
		{ key: 7, name: 'Caroline Schultz' },
		{ key: 8, name: 'Mason Heaney' },
		{ key: 9, name: 'Claudie Smitham' },
		{ key: 10, name: 'Emil Schaefer' },
	]

    const combobox = createCombobox({
        label: "People",
        selected: [people[2]!, people[3]!],
        multi: true,
        onselect(value) {
            console.log("select", value);
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

<div class="fixed top-8 w-full max-w-4xl px-4">
    <div class="relative mt-1">
        <span class="inline-block w-full rounded-md shadow-sm">
            <button
                use:combobox.button
                class="focus:shadow-outline-teal relative w-full cursor-default rounded-md border border-gray-300 bg-white py-2 px-2 text-left transition duration-150 ease-in-out focus:border-teal-300 focus:outline-none text-sm sm:leading-5"
            >
                <div class="flex flex-wrap gap-2">
                    {#each combobox.selected as selected (selected.key)}
                        <span
                            class="flex items-center gap-1 rounded bg-blue-50 px-2 py-0.5"
                        >
                            <span>{selected.name}</span>
                            <div use:combobox.deselect={selected}>
                                <Deselect />
                            </div>
                        </span>
                    {:else}
                        <span
                            class="flex items-center gap-1 rounded px-2 py-0.5"
                        >
                            Empty
                        </span>
                    {/each}
                    <div class="self-stretch my-1 w-0.5 bg-gray-200" />
                    <input
                        use:combobox.input
                        placeholder="Search&hellip;"
                        class="w-auto border-none py-1 grow leading-5 text-gray-900 focus:ring-0 text-sm"
                    />
                    <span
                        class="//pointer-events-none flex items-center px-2 cursor-pointer"
                    >
                        <Selector class="h-5 w-5 text-gray-400" />
                    </span>
                </div>
            </button>
        </span>

        <Transition
            show={combobox.expanded}
            leave="transition ease-in duration-100"
            leaveFrom="opacity-100"
            leaveTo="opacity-0"
        >
            <ul
                use:combobox.items
                class="absolute mt-1 max-h-60 w-full overflow-auto rounded-md bg-white py-1 text-sm shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
            >
                {#each filtered as value (value.key)}
                    {@const active = combobox.isActive(value)}
                    {@const selected = combobox.isSelected(value)}
                    <li
                        class="relative cursor-default select-none py-2 pl-4 pr-9 focus:outline-none {active
                            ? 'bg-teal-600 text-white'
                            : 'text-gray-900'}"
                        use:combobox.item={{ value }}
                    >
                        <span
                            class="block truncate {selected
                                ? 'font-semibold'
                                : 'font-normal'}">{value.name}</span
                        >
                        {#if selected}
                            <span
                                class="absolute inset-y-0 right-0 flex items-center pr-3 {active
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
