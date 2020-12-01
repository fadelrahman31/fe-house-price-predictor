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
              <v-card
                ref="form"
                v-model="valid"
                flat
              >
                <!-- BUTUH ID RUMAH -->
                <h3>Masukkan jumlah kamar tidur</h3>
                <v-text-field
                  v-model="bedrooms"
                  type="number"
                  label="Jumlah kamar tidur, e.g. 0,1,2,.."
                ></v-text-field>
              
                <h3>Masukkan jumlah kamar mandi</h3>
                <v-text-field
                  v-model="bathrooms"
                  type="number"
                  label="Jumlah kamar mandi, e.g. 0,1,2,.."
                ></v-text-field>

                <h3>Masukkan luas bangunan dalam satuan square feet</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="sqft_living"
                  label="Jumlah luas bangunan, e.g. 2540, etc"
                  type="number"
                ></v-text-field>

                <h3>Masukkan luas tanah dalam satuan square feet</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="sqft_lot"
                  label="Jumlah luas tanah, e.g. 2034, etc"
                  type="number"
                ></v-text-field>

                <h3>Masukkan jumlah total lantai di dalam rumah</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="floors"
                  label="Jumlah lantai rumah, e.g. 1,2,.."
                  type="number"
                ></v-text-field>

                <h3>Apakah rumah memiliki pemandangan view ke perairan? (sungat/danau/laut)</h3>
                <!-- berupa boolean 1/0 -->
                <v-select
                  v-model="waterfront"
                  :items="pilWaterfront"
                  label="Jika IYA pilih 1, jika TIDAK pilih 0"
                ></v-select>
                
                <h3>Berapa penilaian view/tampak luar rumah?</h3>
                <!-- berupa option 1-4 -->
                <v-select
                  v-model="view"
                  :items="pilView"
                  label="Isikan sesuai hasil penilaian view rumah anda"
                ></v-select>
                
                <h3>Berapa penilaian kondisi bangunan rumah?</h3>
                <!-- berupa option 1-5 -->
                <v-select
                  v-model="condition"
                  :items="pilCondition"
                  label="Isikan sesuai hasil penilaian kondisi rumah anda"
                ></v-select>

                <h3>Berapa penilaian yang diberikan pada rumah berdasar standar penilaian King County?</h3>
                <!-- berupa otpion 1-13 -->
                <v-select
                  v-model="grade"
                  :items="pilGrade"
                  label="Isikan sesuai hasil penilaian King County"
                ></v-select>

                <h3>Masukkan luas bangunan rumah di atas ground level!</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="sqft_above"
                  label="Luas bangunan di atas level tanah"
                  type="number"
                ></v-text-field>

                <h3>Masukkan luas bangunan rumah di bawah ground level (basement) !</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="sqft_basement"
                  label="Luas bangunan di bawah level tanah"
                  type="number"
                ></v-text-field>

                <h3>Masukkan tahun pembangunan rumah!</h3>
                <!-- berupa date year -->
                <v-text-field
                  v-model="yr_built"
                  label="Tahun bangunan dibangun"
                  type="number"
                ></v-text-field>

                <h3>Masukkan tahun terakhir renovasi rumah!</h3>
                <!-- berupa date year, bisa 0 -->
                <v-text-field
                  v-model="yr_renovated"
                  label="Tahun bangunan direnovasi terakhir kali"
                  type="number"
                ></v-text-field>

                <h3>Masukkan kode pos rumah anda!</h3>
                <!-- integer -->
                <v-text-field
                  v-model="zipcode"
                  label="Kode pos rumah anda"
                ></v-text-field>

                <h3>Masukkan koordinat Latitude rumah anda!</h3>
                <!-- float -->
                <v-text-field
                  v-model="lat"
                  label="Koordinat latitude rumah anda, e.g. 47.5112"
                ></v-text-field>

                <h3>Masukkan koordinat Longitude rumah anda!</h3>
                <!-- float -->
                <v-text-field
                  v-model="long"
                  label="Koordinat longitude rumah anda, e.g. -122.257"
                ></v-text-field>

                <h3>Masukkan luas bangunan dalam satuan square feet pada tahun 2015!</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="sqft_living15"
                  label="Luas bangunan pada tahun 2015"
                  type="number"
                ></v-text-field>

                <h3>Masukkan luas tanah dalam satuan square feet pada tahun 2015!</h3>
                <!-- berupa integer -->
                <v-text-field
                  v-model="sqft_lot15"
                  label="Luas tanah pada tahun 2015"
                  type="number"
                ></v-text-field>

                <!-- BUTUH ATRIBUT DATA DATE, TENTUKAN BENTUKNYA DULU -->
              </v-card>
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
    bedrooms: 0,
    bathrooms: 0,
    sqft_living:0,
    sqft_lot: 0,
    floors: 0,
    waterfront: '',
    pilWaterfront: [0,1],
    view: 0,
    pilView: [1,2,3,4],
    condition: 0,
    pilCondition: [1,2,3,4,5],
    grade:0,
    pilGrade: [1,2,3,4,5,6,7,8,9,10,11,12,13],
    sqft_above: 0,
    sqft_basement: 0,
    yr_built: '',
    yr_renovated: '',
    zipcode: '',
    lat: '',
    long: '',
    sqft_living15: 0,
    sqft_lot15: 0,

  })

}
</script>
