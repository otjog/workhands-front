<template>
	<MainLayout>
		<v-main class="bg-grey-lighten-3">
			<v-container>
				<v-row>
					<v-col cols="12" md="2"></v-col>

					<v-col cols="12" md="8">
						<v-sheet rounded="lg" class="mx-auto" width="700">

							<v-form ref="form">
								<v-text-field
									v-model="name"
									:counter="200"
									label="Name"
									:rules="nameRules"
									:error-messages="nameError"
									required
								></v-text-field>

								<v-text-field
									v-model="price"
									label="Price"
									:rules="priceRules"
									:error-messages="priceError"
									required
								></v-text-field>

								<v-textarea
									label="Description"
									v-model="description"
									:counter="2000"
									:rules="descriptionRules"
									:error-messages="descriptionError"
									name="input-7-1"
									variant="filled"
									auto-grow
								></v-textarea>

								<v-file-input
									label="Photos"
									v-model="photos"
									chips
									multiple
									counter
									accept="image/*"
									:rules="fileRules"
									:error-messages="photosError"
									prepend-icon="mdi-camera"
								></v-file-input>

								<div class="d-flex flex-column">
									<v-btn
										class="mt-4"
										color="success"
										block
										@click="create"
									>
										Create
									</v-btn>
								</div>
							</v-form>
						</v-sheet>
					</v-col>

					<v-col cols="12" md="2"></v-col>
				</v-row>
			</v-container>
		</v-main>
	</MainLayout>
</template>

<script setup>
import MainLayout from "~/layouts/MainLayout.vue";

let form = ref();
let name = ref();
let nameError = ref();
let price = ref();
let priceError = ref();
let description = ref();
let descriptionError = ref();
let photos = ref();
let photosError = ref();

const router = useRouter();

const endpoint = "http://127.0.0.1:8000/api/adverts/";

const nameRules = [
	value => {
		if (value && value.length <= 200)
			return true;
		else if(value && value.length > 200)
			return 'The name field must be less than 200 characters';

		return 'You must enter a first name.';
	}
];

const descriptionRules = [
	value => {
		if (value && value.length <= 2000)
			return true;
		else if(value && value.length > 2000)
			return 'The description field must be less than 2000 characters';
		return 'You must enter a description.';
	}
];

const priceRules = [
	value => {
		if (value > 0 && value <= 999999999)
			return true;
		else if (value && value > 999999999)
			return 'The price must be less than 999999999';
		return 'You must enter a price.';
	}
];

const fileRules = [
	value => {
		if(value.length > 0 && value.length <= 3)
			return true;
		else if(value.length > 3)
			return 'The number of photos must be from 1 to 3';
		return 'You must upload photos.';
	}
];

const create = async () => {
	const {valid} = await form.value.validate()

	if (valid) {

		try {
			let response = await $fetch(endpoint, {
				method: "POST",
				body: {
					name: name.value,
					price: price.value,
					description: description.value,
					photos: setPhotos(photos.value.length)
				}
			});

			await router.push(`/${response.data.id}`)

		} catch (e) {
			setErrors(e.response._data.errors);
		}

	}
}

const setPhotos = (count) => {
	let array = [];

	for(let i = 0; i < count; i++) {
		array[i] = `https://picsum.photos/id/${Math.floor(Math.random()*1000)}/800/800`;
	}

	return array;
}

const setErrors = (errors) => {
	for (let error in errors) {
		let fieldName = error.split('.')[0];

		switch (fieldName) {
			case 'name':
				nameError.value = errors[error];
				break;
			case 'price':
				priceError.value = errors[error];
				break;
			case 'description':
				descriptionError.value = errors[error];
				break;
			case 'photos':
				photosError.value = errors[error];
				break;
		}
	}
}

</script>
