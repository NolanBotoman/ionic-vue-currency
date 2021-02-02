<script>
import { IonButton, IonContent, IonPage, toastController } from '@ionic/vue';

export default {
    components: { IonButton, IonContent, IonPage },
    methods: {
        async openCurrencyToast() {
            const toast = await toastController
                .create({
                    message: 'Veuillez choisir une devise avant de convertir un montant.',
                    duration: 2000
                })
            return toast.present();
        },
        async openNumberToast() {
            const toast = await toastController
                .create({
                    message: 'Vous ne pouvez entrer que des chiffres.',
                    duration: 2000
                })
            return toast.present();
        }
    },
}
</script> 