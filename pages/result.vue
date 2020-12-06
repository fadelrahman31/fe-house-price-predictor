<template>
    <v-app>
        <v-container>
            <v-layout justify-center align-center>
                <v-container fluid>
                    <h1>Prediction Results Status</h1>
                    <p>Berikut merupakan hasil prediksi harga untuk rumah anda!</p>
                    <v-spacer></v-spacer>
                    <v-spacer></v-spacer>
                    <v-row
                        justify="center"
                    >
                        <v-col
                            cols="12"
                            md="10"
                            
                        >
                            <v-simple-table>
                                <template>
                                    <thead>
                                        <tr>
                                            <th class="text-center">
                                                <h2>Information</h2>
                                            </th>
                                            <th class="text-center">
                                                <h2>Detail</h2>
                                            </th>                                        
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td>Status Prediksi</td>
                                            <td><i>{{ this.prediksi }}</i></td>
                                        </tr>
                                        <tr>
                                            <td><h3>Prediksi Harga Rumah</h3></td>
                                            <td><h3>{{ this.harga }}</h3></td>
                                        </tr>                                        
                                    </tbody>
                                </template>
                            </v-simple-table>
                        </v-col>
                    </v-row>
                    <v-spacer></v-spacer>
                    <v-btn
                        text
                        light
                        color="success"
                        @click="goBacktoIndex"
                    >
                        go home
                    </v-btn>
                </v-container>                
            </v-layout>
        </v-container>
    </v-app>
</template>

<script>
export default {
    data: () => ({
        prediksi: '',
        harga : ''
    }),
    methods :{
        goBacktoIndex(){
            this.$router.push({
                  path: '/'
            })
        }
    },
    async mounted() {
        var urlParams = window.location.search
        var urlParams = urlParams.toString()
        var urlParams = urlParams.substring(1)
        console.log(urlParams)
        let params = new URLSearchParams(urlParams)
        
        this.prediksi = params.get('msg')
        this.harga = params.get('prc')
    }
    
}
</script>