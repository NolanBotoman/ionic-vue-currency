<template>
    <ion-col class="flex ion-justify-content-center" size="12">
        <ion-spinner class="ion-hide" name="crescent"></ion-spinner>
        <ion-item class="ion-padding" disabled="true" lines="none">
            <ion-text color="black" v-if="result.converted" class="ion-text-center">
                <h2>{{ result.converted }} {{ result.code }}</h2>
            </ion-text>
            <ion-text color="black" class="ion-text-center" v-else>
                <h4>Entrez un montant et sélectionnez une devise pour débuter une conversion.</h4>
            </ion-text>
        </ion-item>
    </ion-col>
</template>
<script>
    import {
        IonCol,
        IonSpinner,
        IonItem,
        IonText
    } from "@ionic/vue";

    const results = {
        props: ['convert'],
        data() {
            return {
                result: {
                    eur: this.convert.amount,
                    code: this.convert.code,
                    currency: this.convert.currency,
                    converted: this.convert.amount * this.convert.currency
                },
            }
        },
        watch: {
            convert: {
                deep: true,
                handler() {
                    this.result.eur = this.convert.amount;
                    this.result.code = this.convert.code;
                    this.result.currency = this.convert.currency;
                    this.result.converted = Math.round((this.convert.currency * this.convert.amount) * 100) / 100; 
                }
            }
        },
        components: {
            IonCol,
            IonSpinner,
            IonItem,
            IonText
        }
    };

    export default results;
</script>