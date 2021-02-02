<template>
    <ion-col size="12">
        <ion-item class="ion-margin-horizontal ion-padding-left" lines="full">
            <ion-label position="fixed">Amount</ion-label>
            <ion-input class="ion-text-right" @input="buildSearch" @keypress="handleInput" type="text" placeholder="EUR"></ion-input>
        </ion-item>
        <ion-item class="ion-margin-horizontal ion-padding-left" lines="full">
            <ion-label>Currency</ion-label>
            <ion-select interface="action-sheet" @ionChange="handleCurrency">
                <ion-select-option v-for="(value, key) in currencies" :key="key" :value="key">{{ value }}</ion-select-option>
            </ion-select>
        </ion-item>
        <toast ref="Toast"></toast>
    </ion-col>
</template>
<script>
    import {
        IonCol,
        IonToolbar,
        IonItem,
        IonSearchbar,
        IonLabel,
        IonSelect,
        IonSelectOption,
        IonInput
    } from "@ionic/vue";

    import Toast from "./Toast";

    const search = {
        data() {
            return {
                currencies: [],
                selected: "",
                searchAmount: "",
                query: {
                    amount: "",
                    code: "",
                    currency: ""
                },
            }
        },
        methods: {
            buildDevises() {
                fetch(`http://data.fixer.io/api/symbols?access_key=${process.env.VUE_APP_API_KEY}`)
                    .then(res => res.json())
                    .then(data => {
                        this.currencies = data.symbols;
                        this.error = ""
                    })
            },
            buildSearch(event) {
                this.searchAmount = event.target.value;
                this.search(this.searchAmount);
            },
            handleCurrency(event) {
                this.selected = event.target.value;
                this.search(this.searchAmount);
            },
            search(value) {
                fetch(`http://data.fixer.io/api/latest?access_key=${process.env.VUE_APP_API_KEY}&base=EUR&symbols=${this.selected}`)
                    .then(res => res.json())
                    .then(data => {
                        this.query.code = Object.entries(data.rates)[0][0];
                        this.query.currency = Object.entries(data.rates)[0][1];
                        this.query.amount = value;

                        this.$emit('search', this.query);
                    });
            },
            handleInput(event) {
                if (this.isNumber(event)) {
                    event.preventDefault();
                    this.$refs.Toast.openNumberToast();
                } else if (this.isCurrency()) {
                    event.preventDefault();
                    this.$refs.Toast.openCurrencyToast();
                } 
            },
            isNumber(event) {
                event = (event) ? event : window.event;
                var charCode = (event.which) ? event.which : event.keyCode;

                if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
                    return true;
                } else {
                    return false;
                }
            },
            isCurrency() {
                if (this.selected == "") {
                    return true;
                } else {
                    return false;
                }
            }
        },
        mounted() {
            this.buildDevises()
        },
        components: {
            IonCol,
            IonToolbar,
            IonItem,
            IonSearchbar,
            IonLabel,
            IonSelect,
            IonSelectOption,
            IonInput,
            Toast
        }   
    };

    export default search;
</script>