<script setup>
import { convertDate } from '../utils';
</script>

<template>
	<main class="bg-stone-50">
		<div class="mx-auto max-w-xl pb-5">
			<ContentDoc v-slot="{ doc }">
				<h1
					class="mb-8 border-b-2 border-dotted border-gray-500 py-10 text-center text-4xl font-semibold text-gray-900 md:py-16 md:px-24"
				>
					{{ doc.title }}
				</h1>
				<p>{{ convertDate(doc.date) }}</p>
				<div class="prose prose-lg text-black">
					<ContentRenderer :value="doc" />
				</div>
			</ContentDoc>
		</div>
	</main>
</template>
