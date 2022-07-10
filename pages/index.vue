<script setup>
import { convertDate } from '../utils';

const articles = await queryContent()
	.sort({
		date: -1,
		$numeric: true
	})
	.find();

const selectedCategory = ref('');

const filteredCategories = computed(() => {
	return articles.reduce((acc, i) => {
		if (!acc.find((v) => v.category == i.category)) {
			acc.push(i);
		}
		return acc;
	}, []);
});

const displayedArticles = computed(() => {
	if (selectedCategory.value) {
		return articles.filter((article) => article.category.includes(selectedCategory.value));
	} else {
		return articles;
	}
});
</script>

<template>
	<main class="bg-stone-50 px-4 pt-16 pb-20 sm:px-6 lg:px-8 lg:pt-24 lg:pb-28">
		<div class="mx-auto max-w-lg lg:max-w-5xl">
			<div class="border-b-2 border-dotted border-gray-500 pb-5">
				<h2 class="mb-5 text-3xl font-semibold tracking-tight text-gray-900 sm:text-4xl">
					Articles
				</h2>
				<div class="flex flex-wrap">
					<button
						@click="selectedCategory = ''"
						class="m-1 rounded border border-gray-200 py-1 px-2 text-lg font-bold text-blue-700 transition hover:bg-blue-50 focus:bg-blue-700 focus:text-white"
					>
						All
					</button>
					<button
						v-for="article in filteredCategories"
						:key="article._path"
						@click="selectedCategory = article.category"
						class="m-1 rounded border border-gray-200 py-1 px-2 text-lg font-bold text-blue-700 transition hover:bg-blue-50 focus:bg-blue-700 focus:text-white"
					>
						{{ article.category }}
					</button>
				</div>
			</div>
			<div class="mt-8 grid gap-16 lg:grid-cols-2 lg:gap-5">
				<div
					v-for="article in displayedArticles"
					:key="article._path"
					class="flex flex-col justify-between p-4"
				>
					<nuxt-link :href="article._path">
						<h5 class="text-2xl font-semibold text-blue-700">{{ article.title }}</h5>
						<div class="mt-3">
							<div class="text-sm text-gray-700">
								<time datetime="2022-03-16">{{ convertDate(article.date) }}</time>
							</div>
						</div>
						<p class="mt-3">{{ article.description }}</p>
					</nuxt-link>
				</div>
			</div>
		</div>
	</main>
</template>
