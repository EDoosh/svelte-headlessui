<script lang="ts">
    import { Transition, createListbox } from "svelte-headlessui";
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

    const listbox = createListbox({
        label: "Actions",
        selected: [people[2]!, people[3]!],
        multi: true,
        onselect(value) {
            console.log("select", value);
        }
    });
</script>

<div class="fixed top-12 w-full max-w-4xl px-4">
    <div class="relative mt-1">
        <span class="inline-block w-full rounded-md shadow-sm">
            <button
                use:listbox.button
                class="focus:shadow-outline-orange relative w-full cursor-default rounded-md border border-gray-300 bg-white py-2 pl-2 pr-10 text-left transition duration-150 ease-in-out focus:border-orange-300 focus:outline-none text-sm sm:leading-5"
            >
                <div class="flex flex-wrap gap-2">
                    {#each listbox.selected as selected (selected.key)}
                        <span
                            class="flex items-center gap-1 rounded bg-orange-50 px-2 py-0.5"
                        >
                            <span>{selected.name}</span>
                            <div use:listbox.deselect={selected}>
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
                </div>
                <span
                    class="pointer-events-none absolute inset-y-0 right-0 flex items-center pr-2"
                >
                    <Selector class="h-5 w-5 text-gray-400" />
                </span>
            </button>
        </span>

        <Transition
            show={listbox.expanded}
            leave="transition ease-in duration-100"
            leaveFrom="opacity-100"
            leaveTo="opacity-0"
        >
            <ul
                use:listbox.items
                class="absolute mt-1 max-h-60 w-full overflow-auto rounded-md bg-white py-1 text-sm shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
            >
                {#each people as value (value.key)}
                    {@const active = listbox.isActive(value)}
                    {@const selected = listbox.isSelected(value)}
                    <li
                        class="relative cursor-default select-none py-2 pl-4 pr-9 focus:outline-none {active
                            ? 'bg-orange-100 text-orange-900'
                            : 'text-gray-900'}"
                        use:listbox.item={{ value }}
                    >
                        <span
                            class="block truncate {selected
                                ? 'font-semibold'
                                : 'font-normal'}">{value.name}</span
                        >
                        {#if selected}
                            <span
                                class="absolute inset-y-0 right-0 flex items-center pr-3 text-orange-600"
                            >
                                <Check class="h-5 w-5" />
                            </span>
                        {/if}
                    </li>
                {/each}
            </ul>
        </Transition>
    </div>
</div>
