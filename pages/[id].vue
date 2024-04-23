<template>
	<MainLayout>
		<v-main class="bg-grey-lighten-3" v-if="advert">
			<v-container>
				<v-row>
					<v-col cols="12" md="2"></v-col>

					<v-col
						cols="12"
						md="8"
					>
						<v-sheet rounded="lg" class="pb-4">
							<v-img
								class="bg-grey-lighten-2"
								max-height="400"
								:src="mainPhotoUrl"
								cover
							></v-img>
						</v-sheet>

						<v-row v-if="advert.photos">
							<v-col
								v-for="photo in advert.photos"
								:key="photo"
								class="d-flex child-flex"
								cols="4"
							>
								<v-img
									@click="changeMainPhoto(photo.url)"
									:src="photo.url"
									aspect-ratio="1"
									class="bg-grey-lighten-2 cursor-pointer"
									cover
								>
									<template v-slot:placeholder>
										<v-row
											align="center"
											class="fill-height ma-0"
											justify="center"
										>
											<v-progress-circular
												color="grey-lighten-5"
												indeterminate
											></v-progress-circular>
										</v-row>
									</template>
								</v-img>
							</v-col>
						</v-row>

						<h1>{{advert.name}}</h1>

						<div>Price: {{advert.price}}</div>

						<div v-if="advert.description">
							{{advert.description}}
						</div>

					</v-col>

					<v-col cols="12" md="2">    </v-col>
				</v-row>
			</v-container>
		</v-main>
	</MainLayout>
</template>

<script setup>
import MainLayout from "~/layouts/MainLayout.vue";

let advert = ref();

let mainPhotoUrl = ref();

const fields = [
	'description',
	'photos'
];

const endpoint = "http://127.0.0.1:8000/api/adverts/";
const route = useRoute();

onMounted(async () => {
	let response = await $fetch(endpoint + route.params.id + getQuery());

	advert.value = response.data;
	mainPhotoUrl.value = advert.value.main_photo.url;

	console.log(advert.value);
})

const getQuery = () => {

	let string = '';
	let index = 0;

	fields.forEach((fieldName) => {
		if (index > 0)
			string += '&';
		else
			string += '?';
		string += `fields[${index}]=${fieldName}`;
		index++;
	})

	return string;
}

const changeMainPhoto = (url) => {
	console.log('s');
	mainPhotoUrl.value = url;
}

</script>
