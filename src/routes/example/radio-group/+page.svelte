<script lang="ts">
	let group = $state('startup')
</script>

{#snippet radioItem({ id, title, cpu, storage }: { id: string; title: string; cpu: string; storage: string })}
	<div class="relative">
		<input id={id} class="absolute opacity-0 w-0 h-0 peer" type="radio" bind:group name="type" value={id} />
		<label
			for={id}
			class="transition [&_p]:text-gray-900 [&_p]:font-medium [&_span]:text-gray-500 peer-checked:[&_p]:text-white peer-checked:[&_p]:font-bold peer-checked:[&_span]:text-sky-100 peer-focus:ring-2 peer-focus:ring-white peer-focus:ring-opacity-60 peer-focus:ring-offset-2 peer-focus:ring-offset-sky-300 bg-white relative flex cursor-pointer rounded-lg px-5 py-4 shadow-md focus:outline-none peer-checked:bg-sky-900/75 peer-checked:text-white"
		>
			<div class="flex w-full items-center justify-between">
				<div class="flex items-center">
					<div class="text-sm">
						<p class="transition">{title}</p>
						<span class="inline transition"
							><span>{cpu}</span> <span aria-hidden="true">·</span> <span>{storage}</span></span
						>
					</div>
				</div>
				<div class="shrink-0 text-white transition">
					<svg viewBox="0 0 24 24" fill="none" class="h-6 w-6">
						<circle cx="12" cy="12" r="12" fill="#fff" opacity="0.2" /><path
							d="M7 13l3 3 7-7"
							stroke="#fff"
							stroke-width="1.5"
							stroke-linecap="round"
							stroke-linejoin="round"
						/>
					</svg>
				</div>
			</div>
		</label>
	</div>
{/snippet}

<div class="w-full px-4 py-16">
	<div class="mx-auto w-full max-w-md">
		<fieldset class="flex flex-col space-y-2">
			{@render radioItem({ id: "startup", title: "Startup", cpu: "12GB / 6 CPUs", storage: "160 GB SSD disk" })}
			{@render radioItem({ id: "business", title: "Business", cpu: "16GB / 8 CPUs", storage: "512 GB SSD disk" })}
			{@render radioItem({ id: "enterprise", title: "Enterprise", cpu: "32GB / 12 CPUs", storage: "1024 GB SSD disk" })}
		</fieldset>
	</div>
</div>
