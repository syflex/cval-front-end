<template>
    <div>
        <q-btn no-caps label="Signature" @click="opened = true" />


        <q-modal v-model="opened" :content-css="{minWidth: '80vw', minHeight: '80vh'}">
        <q-modal-layout>
            <q-toolbar slot="header">
            <q-btn flat round dense v-close-overlay  icon="keyboard_arrow_left" />
            <q-toolbar-title>
                Capture Signature
            </q-toolbar-title>
            </q-toolbar>

            <div class="layout-padding">

            <q-btn no-caps label="Accept & sign" @click="signature = true"/>

            <div v-if="signature">
                <VueSignaturePad width="100%" height="200px" ref="signaturePad" class="bg-grey-5" />
                <div>
                    <button @click="save">Save</button>
                    <button @click="undo">Undo</button>
                </div>
            </div>

    </div>
  </q-modal-layout>
</q-modal>


    </div>
</template>

<script>
// import signature from './partials/signature'
import { QUploader } from "quasar";
export default {
    props:['signature_p'],

    data(){
        return{
            opened: false,            
            signature: this.signature_p ? signature_p : false,
        }
    },

    components:{
        QUploader
    },
    
    methods:{
       
    undo() {
        this.$refs.signaturePad.undoSignature();
      },

      save() {
        const { isEmpty, data } = this.$refs.signaturePad.saveSignature();
        console.log(isEmpty);
        console.log(data);
        this.$emit('clicked', data)
      },
    },

    mounted () {
        
    }

}
</script>

<style>

</style>
