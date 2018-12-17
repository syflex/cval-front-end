<template>
    <div>
        <div class="row full-width gutter-sm">
            <div class="col-7">
                <q-input v-model="form.claimant_id" placeholder="Claimant ID"/>
                <q-input v-model="form.first_name" placeholder="Claimant First Name"/>
                <q-input v-model="form.last_name" placeholder="Claimant Last Name"/>
                <q-input v-model="form.other_name" placeholder="Claimant Other Names"/>
                <q-input v-model="form.location" placeholder="Claimant Location"/>
                <q-input v-model="form.community" placeholder="Claimant Community"/>
                <q-input v-model="form.coordinates" placeholder="Claimant Coodinates"/>                
            </div>
            <div class="col-5">
                <q-uploader url="url" extensions=".jpg,.jpeg,.png" auto-expand 
                    :upload-factory="uploadFile"
                />
                <!-- <q-uploader url="url" /> -->
                <!-- <signature/> -->
                <q-btn no-caps label="Signature" @click="signature = true"/>
            </div>
        </div>
        
        <div v-if="signature">
            <VueSignaturePad
                width="500px"
                height="200px"
                ref="signaturePad" class="bg-grey-5"
                />
            <div>
            <button @click="save">Save</button>
            <button @click="undo">Undo</button>
            </div>
        </div>
        

    <div v-for="(crop, index) in form.crops" v-bind:key="index" class="row">
        <q-field class="col-2">
            <q-select v-model="crop.type" float-label="Crop Type" :options="typeOptions"
                @input="filterType(crop.type)"
            />
        </q-field>
        <q-field class="col-2">
            <q-select v-model="crop.name" float-label="Crop Name" :options="crops" />
        </q-field> 
        <q-field class="col-2">
            <q-select v-model="crop.maturity" float-label="Crop Maturity" :options="maturityOptions" 
                @input="check(crop.name,crop.maturity,index)"
            />
        </q-field>        
                
        <q-field class="col-2">
            <q-input v-model="crop.unit" float-label="Quantit eg(5)" numeric-keyboard-toggle placeholder="Quantity eg(5)"/>
        </q-field>
        <q-field class="col-1">
            <q-input v-model="crop.price" float-label="Rate (N)" numeric-keyboard-toggle placeholder="Rate (N)"/>
        </q-field>
        <q-field class="col-2">
            <q-input :value="crop.value = (Number(crop.unit) * Number(crop.price))" float-label="Rate (N)" numeric-keyboard-toggle placeholder="Rate (N)"/>
        </q-field>

        <div class="col-lg-1">
            <div class="float-center row flex-center">
                <q-btn color="red" size="sm" @click="removeLine(index)" icon="delete" round/>
                <q-btn color="secondary" size="sm" v-if="index + 1 === form.crops.length" @click="addLine" icon="add" round />
            </div>
        </div>
    </div>

        <div class="row full-width">
            <q-btn class="full-width" @click="save_entery" color="secondary" label="Save Entery" :loading="loading" no-caps>
                    <q-spinner-ios slot="loading" class="q-mr-sm"></q-spinner-ios>
                    <span slot="loading"> Saving Entery..</span>
            </q-btn>  
        </div>

    </div>
</template>

<script>
// import signature from './partials/signature'
import { QUploader } from "quasar";
import add from "./partials/add"
export default {

    data(){
        return{
            form:{
                claimant_id: 'NNPC/MCO/DBS/005435',
                first_name: null,
                last_name: null,
                other_name: null,
                location: null,
                community: null,
                coordinates: null,
                c_signature: null,

                crops: [],

            },
            loading: false,
            opened: false,
            filter_type: null,
            crops: [],
            signature: false,

            typeOptions:[
                    {
                        label: 'Economic Trees', value: 'Economic Trees'
                    }, 
                    {
                        label: 'Crops', value: 'Crops'
                    }
                ],          

            maturityOptions:[
                    {
                        label: 'Mature', value: 'Mature'
                    }, 
                    {
                        label: 'Immature', value: 'Immature'
                    }, 
                    {
                        label: 'Seedling', value: 'Seedling'
                    }
                ],
                
            mainData:[
                    {
                        type: 'Economic Trees', 
                        data: [
                            {
                                label: 'Mango', value: 'Mango'
                            }, 
                            {
                                label: 'Dorowa', value: 'Dorowa'
                            },
                            {
                                label: 'Dabino', value: 'Dabino'
                            }, 
                            {
                                label: 'Gamji', value: 'Gamji'
                            },
                            {
                                label: 'Kadauya', value: 'Kadauya'
                            }, 
                            {
                                label: 'Rimi', value: 'Rimi'
                            }
                        ]
                    }, 
                    {
                        type: 'Crops', 
                        data: [
                            {
                                label: 'Millet', value: 'Millet'
                            }, 
                            {
                                label: 'Guinea Corn', value: 'Guinea Corn'
                            },
                            {
                                label: 'Maize', value: 'Maize'
                            }, 
                            {
                                label: 'Rice', value: 'Rice'
                            },
                            {
                                label: 'Beans', value: 'Beans'
                            }, 
                            {
                                label: 'Groundnut', value: 'Groundnut'
                            }
                        ]
                    }
                ],

            dataPrice:[
                    {
                        crop: 'Mango', 
                        data: [
                            {label: 'Mature', value: '10000'},{label: 'Immature', value: '5000'},{label: 'Seedling', value: '2500'}
                        ]
                    }, 
                    {
                        crop: 'Dorowa', 
                        data: [
                            {label: 'Mature', value: '10000'},{label: 'Immature', value: '5000'},{label: 'Seedling', value: '2500'}
                        ]
                    },
                    {
                        crop: 'Dabino', 
                        data: [
                            {label: 'Mature', value: '10000'},{label: 'Immature', value: '5000'},{label: 'Seedling', value: '2500'}
                        ]
                    },
                    {
                        crop: 'Gamji', 
                        data: [
                            {label: 'Mature', value: '10000'},{label: 'Immature', value: '5000'},{label: 'Seedling', value: '2500'}
                        ]
                    },
                    {
                        crop: 'Kadauya', 
                        data: [
                            {label: 'Mature', value: '10000'},{label: 'Immature', value: '5000'},{label: 'Seedling', value: '2500'}
                        ]
                    },
                    {
                        crop: 'Rimi', 
                        data: [
                            {label: 'Mature', value: '5000'},{label: 'Immature', value: '2500'},{label: 'Seedling', value: '1250'}
                        ]
                    },

                    {
                        crop: 'Millet', 
                        data: [
                            {label: 'Mature', value: '60000'},{label: 'Immature', value: '30000'},{label: 'Seedling', value: '15000'}
                        ]
                    }, 
                    {
                        crop: 'Guinea Corn', 
                        data: [
                            {label: 'Mature', value: '60000'},{label: 'Immature', value: '30000'},{label: 'Seedling', value: '15000'}
                        ]
                    },
                    {
                        crop: 'Maize', 
                        data: [
                            {label: 'Mature', value: '80000'},{label: 'Immature', value: '40000'},{label: 'Seedling', value: '20000'}
                        ]
                    },
                    {
                        crop: 'Rice', 
                        data: [
                            {label: 'Mature', value: '100000'},{label: 'Immature', value: '50000'},{label: 'Seedling', value: '25000'}
                        ]
                    },
                    {
                        crop: 'Beans', 
                        data: [
                            {label: 'Mature', value: '80000'},{label: 'Immature', value: '40000'},{label: 'Seedling', value: '20000'}
                        ]
                    },
                    {
                        crop: 'Groundnut', 
                        data: [
                            {label: 'Mature', value: '100000'},{label: 'Immature', value: '50000'},{label: 'Seedling', value: '25000'}
                        ]
                    },
                ],
               

        }
    },

    components:{
        QUploader,add
    },
    
    methods:{
        save_entery(){
            this.loading = true;
            this.$axios.post('api/crop', this.form)
            .then(Response => {

                this.$q.notify({color: 'secondary', icon: 'done',
                        message: 'Crop Entry saved successfully'
                    })
                    this.loading = false;
                    this.opened = false   

            })
            .catch(err => {
                    this.loading = false;
                    this.opened = false  
            })
        },

        filterType(value){
            // console.log(value)
            var newData = this.mainData.filter(function (data) {
               return data.type === value
            })

            newData.forEach(element => {
                this.crops = element.data
            });
            
        },

        check(name,maturity,index){
            
            var price;
            var newData = this.dataPrice.filter(function (data) {
               return data.crop === name
            })
            
             var newData1 =  newData[0].data
           
            newData1.forEach(element => {
               if(element.label == maturity){
                  price = element.value
               }
            });

            this.form.crops[index].price = price
        },

        

        addLine () {
            let checkEmptyLines = this.form.crops.filter(crop => crop.value === null)
                if (checkEmptyLines.length >= 2 && this.form.crops.length > 0) return
                this.form.crops.push({
                    type: null,
                    name: null,
                    maturity: null,
                    unit: null,
                    price: null,
                    value: null,
                })
        },

        removeLine (lineId) {
            if (lineId > 0) this.form.crops.splice(lineId, 1)
        },

        undo() {
        this.$refs.signaturePad.undoSignature();
      },
      save() {
        const { isEmpty, data } = this.$refs.signaturePad.saveSignature();
        console.log(isEmpty);
        console.log(data);
        this.form.c_signature = data
      },

      uploadFile (file) {
         
      // "file" is an Object containing file's props, including content

      // for updating progress (as 0-1 floating number), we need to call:
      // updateProgress (bytesTransferred / totalBytes)

      // we need to return a Promise
      // (resolves when upload is done, rejects when there's an error)
    }


    },

    mounted () {
        for (let index = 0; index < 3; index++) {
            this.form.crops.push({
                type: null,
                name: null,
                maturity: null,
                unit: null,
                price: null,
                value: null,
            })     
        }        
    }

}
</script>

<style>

</style>
