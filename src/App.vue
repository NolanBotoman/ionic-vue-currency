<template>
	<ion-app>
		<ion-header class="ion-no-border" translucent>
			<ion-toolbar>
				<ion-title size="small">Currency Converter</ion-title>
			</ion-toolbar>
		</ion-header>
		<ion-content>
			<ion-grid style="height: 100%;">
				<ion-row class="full-size" id="app">
					<search @search="manageQuery"></search>
					<results :convert="query"></results>
				</ion-row>
			</ion-grid>
		</ion-content>
	</ion-app>
</template>
<script>
	import {
		IonApp,
		IonHeader,
		IonToolbar,
		IonTitle,
		IonContent,
		IonGrid,
		IonRow
	} from "@ionic/vue";

	import Search from "./components/Search.vue";
	import Results from "./components/Results.vue";

	const app = {
		data() {
			return {
				query: {},
			}
		},
		methods: {
			manageQuery(query) {
                this.query = query;
            },
		},
		components: {
			IonApp,
			IonHeader,
			IonToolbar,
			IonTitle,
			IonContent,
			IonGrid,
			IonRow,
			Search,
			Results,
		}
	};

	export default app;
</script>