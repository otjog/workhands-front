<template xmlns="http://www.w3.org/1999/html">
	<v-app id="inspire">
		<v-app-bar
			class="px-3"
			density="compact"
			flat
		>
			<v-spacer></v-spacer>

			<v-tabs
				color="grey-darken-2"
				centered
			>
				<v-tab href="/">Main</v-tab>
				<v-tab href="/create">Create</v-tab>
			</v-tabs>
			<v-spacer></v-spacer>

		</v-app-bar>

		<slot/>
	</v-app>


</template>
