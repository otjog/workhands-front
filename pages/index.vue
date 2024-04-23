<template>
	<MainLayout>
		<v-main class="bg-grey-lighten-3" v-if="data">
			<v-container>
				<v-row>
					<v-col
						v-for="advert in data"
						:key="advert"
						cols="12"
						sm="6"
						md="4"
						lg="3"
					>
						<v-card
							class="mx-auto"
							:href="`/${advert.id}`"
						>
							<v-img
								height="400px"
								:src="advert.main_photo.url"
								cover
							></v-img>

							<v-card-title>
								{{advert.name}}
							</v-card-title>
							<v-card-subtitle>
								Цена: {{advert.price}}
							</v-card-subtitle>
						</v-card>
					</v-col>
				</v-row>

			</v-container>
			<v-btn block v-if="cursor" @click="loadAdverts()">Загрузить ещё</v-btn>
		</v-main>
	</MainLayout>
</template>

<script setup>

import MainLayout from "~/layouts/MainLayout.vue";

let data = ref([]);
let cursor = ref();
const endpoint = "http://127.0.0.1:8000/api/adverts/";

onMounted(async () => {
	cursor.value = endpoint;
	await loadAdverts();
})

const loadAdverts = async () => {
	if (cursor.value) {
		let response = await $fetch(cursor.value);
		data.value.push(...response.data);
		cursor.value = response.links.next;
	}
}

</script>
