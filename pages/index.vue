<template>
  <v-app>
    <v-container>
      
      <v-layout justify-center align-center>
        <v-container fluid>
          <h1>Selamat Datang!</h1>
          <p>Masukkan informasi rumah anda untuk dapat memperoleh prediksi harga rumah secara <i>real-time</i> !</p>
          <v-row
            justify="center"
          >
            <v-col
              cols="12"
              md="7"
            >
              <v-expansion-panels focusable>
                <v-expansion-panel>
                  <v-expansion-panel-header>
                    <b>Bagaimana cara menggunakan sistem ini?</b>
                  </v-expansion-panel-header>
                  <v-expansion-panel-content>
                    <p>1. Klik pada tombol <b>Form Input Data Rumah</b> !</p>
                    <p>2. Isi seluruh kolom data mengenai rumah anda! Pastikan seluruh kolom terisi</p>
                    <p>3. Jika telah mengisi seluruh data, Anda dapat memastikan ulang isian anda pada box <i>Summary Input Info</i> !</p>
                    <p>4. Apabila tombol <b>Submit and Predict Now!</b> sudah berwarna hijau, klik tombol tersebut untuk memulai proses prediksi!</p>
                    <p>5. Tunggu hingga proses selesai dan anda akan menuju <i>page</i> baru berisi <b>hasil prediksi harga</b> rumah Anda!</p>
                  </v-expansion-panel-content>
                </v-expansion-panel>
                <v-expansion-panel>
                  <v-expansion-panel-header>
                    <b>Form Input Data Rumah</b>
                  </v-expansion-panel-header>
                  <v-expansion-panel-content>
                    <v-card
                      class="pa-3"
                      flat
                    >
                      <v-form
                        ref="form"
                        v-model= "valid"
                      >
                        <h3>Masukkan jumlah kamar tidur</h3>
                        <v-text-field
                          v-model="bedrooms"
                          :rules="rulesData"
                          label="Jumlah kamar tidur, e.g. 0,1,2,.."
                          type="number"
                          required
                        ></v-text-field>
                      
                        <h3>Masukkan jumlah kamar mandi</h3>
                        <v-text-field
                          v-model="bathrooms"
                          :rules="rulesData"
                          type="number"
                          label="Jumlah kamar mandi, e.g. 0,1,2,.."
                          required
                        ></v-text-field>

                        <h3>Masukkan luas bangunan dalam satuan square feet</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="sqft_living"
                          :rules="rulesData"
                          label="Jumlah luas bangunan, e.g. 2540, etc"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan luas tanah dalam satuan square feet</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="sqft_lot"
                          :rules="rulesData"
                          label="Jumlah luas tanah, e.g. 2034, etc"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan jumlah total lantai di dalam rumah</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="floors"
                          :rules="rulesData"
                          label="Jumlah lantai rumah, e.g. 1,2,.."
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Apakah rumah memiliki pemandangan view ke perairan? (sungat/danau/laut)</h3>
                        <!-- berupa boolean 1/0 -->
                        <v-select
                          v-model="waterfront"
                          :rules="rulesData"
                          :items="pilWaterfront"
                          label="Jika IYA pilih 1, jika TIDAK pilih 0"
                          required
                        ></v-select>
                        
                        <h3>Berapa penilaian view/tampak luar rumah?</h3>
                        <!-- berupa option 1-4 -->
                        <v-select
                          v-model="view"
                          :rules="rulesData"
                          :items="pilView"
                          label="Isikan sesuai hasil penilaian view rumah anda"
                          required
                        ></v-select>
                        
                        <h3>Berapa penilaian kondisi bangunan rumah?</h3>
                        <!-- berupa option 1-5 -->
                        <v-select
                          v-model="condition"
                          :rules="rulesData"
                          :items="pilCondition"
                          label="Isikan sesuai hasil penilaian kondisi rumah anda"
                          required
                        ></v-select>

                        <h3>Berapa penilaian yang diberikan pada rumah berdasar standar penilaian King County?</h3>
                        <!-- berupa otpion 1-13 -->
                        <v-select
                          v-model="grade"
                          :rules="rulesData"
                          :items="pilGrade"
                          label="Isikan sesuai hasil penilaian King County"
                          required
                        ></v-select>

                        <h3>Masukkan luas bangunan rumah di atas ground level!</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="sqft_above"
                          :rules="rulesData"
                          label="Luas bangunan di atas level tanah"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan luas bangunan rumah di bawah ground level (basement) !</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="sqft_basement"
                          :rules="rulesData"
                          label="Luas bangunan di bawah level tanah"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan tahun pembangunan rumah!</h3>
                        <!-- berupa date year -->
                        <v-text-field
                          v-model="yr_built"
                          :rules="rulesData"
                          label="Tahun bangunan dibangun"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan tahun terakhir renovasi rumah!</h3>
                        <!-- berupa date year, bisa 0 -->
                        <v-text-field
                          v-model="yr_renovated"
                          :rules="rulesData"
                          label="Tahun bangunan direnovasi terakhir kali"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan kode pos rumah anda!</h3>
                        <!-- integer -->
                        <v-text-field
                          v-model="zipcode"
                          :rules="rulesData"
                          label="Kode pos rumah anda"
                          required
                        ></v-text-field>

                        <h3>Masukkan koordinat Latitude rumah anda!</h3>
                        <!-- float -->
                        <v-text-field
                          v-model="lat"
                          :rules="rulesData"
                          label="Koordinat latitude rumah anda, e.g. 47.5112"
                          required
                        ></v-text-field>

                        <h3>Masukkan koordinat Longitude rumah anda!</h3>
                        <!-- float -->
                        <v-text-field
                          v-model="long"
                          :rules="rulesData"
                          label="Koordinat longitude rumah anda, e.g. -122.257"
                          required
                        ></v-text-field>

                        <h3>Masukkan luas bangunan dalam satuan square feet pada tahun 2015!</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="sqft_living15"
                          :rules="rulesData"
                          label="Luas bangunan pada tahun 2015"
                          required
                          type="number"
                        ></v-text-field>

                        <h3>Masukkan luas tanah dalam satuan square feet pada tahun 2015!</h3>
                        <!-- berupa integer -->
                        <v-text-field
                          v-model="sqft_lot15"
                          :rules="rulesData"
                          label="Luas tanah pada tahun 2015"
                          required
                          type="number"
                        ></v-text-field>
                      </v-form>
                      <!-- BUTUH ID RUMAH -->
                      

                      <!-- BUTUH ATRIBUT DATA DATE, TENTUKAN BENTUKNYA DULU -->
                    </v-card>
                  </v-expansion-panel-content>
                </v-expansion-panel>
              </v-expansion-panels>
            </v-col>
            <v-spacer></v-spacer>
            <v-col
              cols="12"
              md="5"
            >
              <v-card
                raised
                class="pa-3"
              >
                <h2>Summary Input Info</h2>
                <p>Masukan pengguna untuk setiap atribut</p>
                <v-simple-table
                  v-model="valid"
                >
                  <template>
                    <thead>
                      <tr>
                        <th class="text-center">
                            <h2><i>attribute</i></h2>
                        </th>
                        <th class="text-center">
                            <h2><i>value</i></h2>
                        </th>                                        
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>bedrooms</td>
                        <td>{{ this.bedrooms }}</td>
                      </tr>
                      <tr>
                        <td>bathrooms</td>
                        <td>{{ this.bathrooms }}</td>
                      </tr>
                      <tr>
                        <td>sqft_living</td>
                        <td>{{ this.sqft_living }}</td>
                      </tr>
                      <tr>
                        <td>sqft_lot</td>
                        <td>{{ this.sqft_lot }}</td>
                      </tr>
                      <tr>
                        <td>floors</td>
                        <td>{{ this.floors }}</td>
                      </tr>
                      <tr>
                        <td>waterfront</td>
                        <td>{{ this.waterfront }}</td>
                      </tr>
                      <tr>
                        <td>view</td>
                        <td>{{ this.view }}</td>
                      </tr>
                      <tr>
                        <td>condition</td>
                        <td>{{ this.condition }}</td>
                      </tr>
                      <tr>
                        <td>grade</td>
                        <td>{{ this.grade }}</td>
                      </tr>
                      <tr>
                        <td>sqft_above</td>
                        <td>{{ this.sqft_above }}</td>
                      </tr>
                      <tr>
                        <td>sqft_basement</td>
                        <td>{{ this.sqft_basement }}</td>
                      </tr>
                      <tr>
                        <td>yr_built</td>
                        <td>{{ this.yr_built }}</td>
                      </tr>
                      <tr>
                        <td>yr_renovated</td>
                        <td>{{ this.yr_renovated }}</td>
                      </tr>
                      <tr>
                        <td>zipcode</td>
                        <td>{{ this.zipcode }}</td>
                      </tr>
                      <tr>
                        <td>lat</td>
                        <td>{{ this.lat }}</td>
                      </tr>
                      <tr>
                        <td>long</td>
                        <td>{{ this.long }}</td>
                      </tr>
                      <tr>
                        <td>sqft_living15</td>
                        <td>{{ this.sqft_living15 }}</td>
                      </tr>
                      <tr>
                        <td>sqft_lot15</td>
                        <td>{{ this.sqft_lot15 }}</td>
                      </tr>
                      
                    </tbody>
                  </template>
                </v-simple-table>
                <v-divider></v-divider>
                <v-btn
                  color="success"
                  class="ma-3"
                  @click="submitForm"
                  :loading="submitting"
                  :disabled="!valid"
                >
                  Submit and predict now!
                </v-btn>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    submitting: false,
    bedrooms: '',
    bathrooms: '',
    sqft_living:'',
    sqft_lot: '',
    floors: '',
    waterfront: '',
    pilWaterfront: ['0','1'],
    view: '',
    pilView: ['0','1','2','3','4'],
    condition: '',
    pilCondition: [1,2,3,4,5],
    grade:'',
    pilGrade: [1,2,3,4,5,6,7,8,9,10,11,12,13],
    sqft_above: '',
    sqft_basement: '',
    yr_built: '',
    yr_renovated: '',
    zipcode: '',
    lat: '',
    long: '',
    sqft_living15: '',
    sqft_lot15: '',
    rulesData: [
      v => !!v || 'Field ini wajib diisi'
    ],
    message : '',
    predictedPrice : ''
  }),
  methods: {
    submitForm() {
      

      const formData = new FormData()
      formData.append('bedrooms', this.bedrooms.toString())
      formData.append('bathrooms',this.bathrooms.toString())
      formData.append('sqft_living' , this.sqft_living.toString())
      formData.append('sqft_lot' , this.sqft_lot.toString())
      formData.append('floors' , this.floors.toString())
      formData.append('waterfront' , this.waterfront.toString()) 
      formData.append('view' , this.view.toString())
      formData.append('condition' , this.condition.toString()) 
      formData.append('grade', this.grade.toString())
      formData.append('sqft_above' , this.sqft_above.toString())
      formData.append('sqft_basement' , this.sqft_basement.toString())
      formData.append('yr_built' , this.yr_built.toString())
      formData.append('yr_renovated', this.yr_renovated.toString())
      formData.append('zipcode' , this.zipcode.toString()) 
      formData.append('lat' , this.lat.toString()) 
      formData.append('long', this.long.toString()) 
      formData.append('sqft_living15' , this.sqft_living15.toString())
      formData.append('sqft_lot15' , this.sqft_lot15.toString())
      
       const data = [...formData.entries()]
            console.log("=== Ini Post Query ===")
            console.log(data);

      this.submitting = true;

      this.$axios.post('http://139.59.109.6:5000/predict', formData)
        .then((response) => {
          //console.log(response)

          this.message = response['data']['message']
          this.predictedPrice = response['data']['result']['price']
          
          console.log(this.message)
          console.log(this.predictedPrice)

          let params = new URLSearchParams()
          params.set('msg', this.message)
          params.set('prc', this.predictedPrice)

          console.log(params.toString())
          this.$router.push({
             path: '/result?' + params.toString()
          })

        })
        .catch((e) => {
          console.log("API Error")
          console.log(e)
          this.submitting=true
        })
        .finally(() => {
          this.submitting=true
        })
    }
  }

}
</script>
