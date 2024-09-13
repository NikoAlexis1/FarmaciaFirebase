<template>
    <div>
        <h1>Farmacia</h1>
        <form @submit.prevent="agregarMedicamento">
            <input type="text" placeholder="Ingresa el medicamento" v-model="nuevoMedicamento">
            <button type="submit">Agregar</button>
        </form>
        <ul>
            <li v-for="medicamento in medicamentos">
                {{ medicamento.nombre }}
            </li>
        </ul>
    </div>
</template>
<script>
import { getFirestore, collection, onSnapshot, addDoc, doc, deleteDoc } from 'firebase/firestore';
import firebaseApp from '../firebaseConfig';
export default {
    data() {
        return {
            nuevoMedicamento: '',
            medicamentos: []
        }
    },
    mounted() {
        const db = getFirestore(firebaseApp)
        const medicamentos = collection(db, 'farmacia')
        onSnapshot(medicamentos, (querySnapshot) => {
            querySnapshot.forEach((doc) => {
                this.medicamentos = querySnapshot.docs.map((doc) => ({ 
                    id: doc.id,
                     ...doc.data()
                     }));
            });
        })
    },
    methods: {
        async agregarMedicamento() {
            if(this.nuevoMedicamento === '') return
            const db = getFirestore(firebaseApp)
            const medicamentos = collection(db, 'farmacia')
            await addDoc(medicamentos, {nombre: this.nuevoMedicamento})
            this.nuevoMedicamento = '';
        }
    }
}
</script>