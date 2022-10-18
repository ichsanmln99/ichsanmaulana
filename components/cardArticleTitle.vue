<template>
	<a
		:href="path"
		class="flex-col flex gap-4 lg:flex-row lg:justify-between bg-neutral px-6 py-4 hover:bg-primary hover:text-white active:bg-primary-focus rounded"
	>
		<div class="text-lg line-clamp-1">
			{{ title }}
		</div>
		<div v-if="createdAt" class="flex-none text-sm opacity-60">
			{{ $moment(createdAt).format('DD MMM YY') }}
		</div>
	</a>
</template>

<script>
export default {
	props: {
		title: {
			type: String,
			required: true,
			default: null,
		},
		path: {
			type: String,
			required: true,
			default: null,
		},
		createdAt: {
			type: String,
			required: false,
			default: null,
		},
	},
}
</script>
