<template>
	<div class="page-container">
		<h1>{{ currentCatalog.title }}</h1>
		<div class="scroll-wrap">
			<div class="img-content">
				<simg v-for="(item, index) in currentCatalog.children" :key="index" :src="item.srcPath" :title="item.title"
					@click="handleDetail(item, index)" />
			</div>
		</div>
	</div>
</template>

<script setup>
import simg from './components/s-img.vue';
import { useRoute, useRouter } from 'vue-router';
import { catalogConfig, worksConfig } from '@/config';
import { computed } from 'vue';

const route = useRoute();
const router = useRouter();

const { query } = route;
const currentCatalog = computed(() => {
	const { title, pageType } = query;
	const defaultResult = { title };
	if (pageType == 'catalog') {
		return catalogConfig[title] ?? defaultResult;
	}
	return worksConfig[title] ?? defaultResult;
});

function handleDetail(item, index) {
	const { pageType } = item;

	const handle = {
		catalog: () => {
			router.push({ path: '/catalog', query: { title: item.title, pageType } });
		},
		works: () => {
			router.push({ path: '/catalog', query: { title: item.title, pageType } });
		},
		detail: () => {
			router.push({ path: '/detail', query: { works: query.title, no: index } });
		},
	};
	try {
		handle[pageType]();
	} catch (err) {
		console.log(`错误：${err}`);
	}
}
</script>

<style lang="less" scoped>
.page-container {
	padding-top: var(--page-top);
	padding-left: var(--page-left);
	height: 100%;
	display: flex;
	flex-direction: column;
}
h1{
	margin-left: clamp(120px, 11.88vw, 384px);
}
.scroll-wrap {
	overflow: auto;
	flex:1;
	margin-left: 11.88vw;
	&::-webkit-scrollbar {
		display: none;
	}
}

.img-content {
	display: flex;
	flex-wrap: wrap;
	width: 1280px;
	> * {
		margin-right: var(--col-gap);
		margin-bottom: var(--row-gap);
	}
}

/* 设备宽度大于 320px 小于 640px */
@media all and (min-width: 320px) and (max-width: 640px) {
	.img-content {
		flex-wrap: nowrap;
		flex-direction: column;
	}
	.scroll-wrap{
		margin-left: 0;
	}
}
</style>
