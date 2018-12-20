<template>
    <div>
        <q-btn no-caps color="tertiary" label="Capture Signature" @click="opened = true" />


        <q-modal v-model="opened" :content-css="{minWidth: '80vw', minHeight: '80vh'}">
        <q-modal-layout>
            <q-toolbar slot="header">
            <q-btn flat round dense v-close-overlay  icon="keyboard_arrow_left" />
            <q-toolbar-title>
                Capture Signature
            </q-toolbar-title>
            </q-toolbar>

            <div class="layout-padding">
                <p>this is to confirm that Messrs Muhammad & Co. staff visited my farmland and carried out the enumeration
                    of the above stated crops and economics trees in my presence/attorney and the claims above have been read 
                    and translated to me in my local dialect which i have agreed and consented. I hereby endorse this document
                    to confirm the true position of my claims.
                </p>
            <q-btn no-caps color="tertiary" label="Accept & sign" @click="signature = true" class="full-width"/>
            <q-btn no-caps color="tertiary" label="Accept & thumb print" @click="fingerPrint = true" class="full-width"/>

            <div v-if="signature">
                <VueSignaturePad width="100%" height="200px" ref="signaturePad" class="bg-grey-5" />
                <div>
                    <q-btn color="tertiary" @click="save">Save</q-btn>
                    <q-btn color="negative" @click="undo">Undo</q-btn>
                </div>
            </div>
            <div v-if="fingerPrint">
                <div class="row flex-center">
                    <div style="max-width: 200px" class="bg-white">
                        <img :src="finger" width="50%">     
                        <span v-if="loading" color="white" :loading="loading" no-caps>
                                <q-spinner-ios slot="loading" class="q-mr-sm"></q-spinner-ios>                                
                        </span>                 
                    </div>
                </div>
                <div>
                    <q-btn color="tertiary" @click="enrole">Enroll FingerPrint</q-btn>
                    <q-btn color="tertiary" @click="saveFinger">Save Finger Print</q-btn>
                    <q-btn color="negative" @click="finger = null">Undo</q-btn>
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
import fingers from "./../../../data/fingers.js"
export default {
    props:['signature_p'],

    data(){
        return{
            opened: false,            
            signature: this.signature_p ? signature_p : false,
            fingerPrint: false,
            loading: false,
            fingersList: fingers,
            finger: null,
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
      enrole(){
        this.loading = true
        setTimeout(() => {
              this.loading = false
              this.finger = this.fingersList[Math.floor(Math.random() * this.fingersList.length)];
            }, 3500)
      },

      saveFinger() {
        this.$emit('clicked', this.finger)
    },


    },

    mounted () {
        
    }

}
</script>

<style>

</style>
