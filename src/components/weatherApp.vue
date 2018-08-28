<template>
    <v-container fluid gird-list-xl>
        <v-layout row wrap>
            <v-flex xs12 sm6 d-flex align-center justify-center class="pa-3">
                <section>
                    <v-form ref="form" v-model="valid" lazy-validation required>
                        <v-text-field label="Enter City, Country Code (eg. Toronto, CA)" prepend-icon="place" v-model="city" :rules="empty"></v-text-field>
                        <v-btn @click="submit" class="cta-btn">Search</v-btn>
                        <v-btn @click="clear" class="cta-btn">clear</v-btn>
                    </v-form>
                </section>
            </v-flex>
            <v-flex xs12 sm6 class="pa-3">
                <v-card class="pa-3 weather-board" v-if="cityName[0]">
                    <v-layout row>
                        <v-flex xs6>
                            <h2 class="card-title">{{cityName}}, {{country.country}}</h2><br> <img v-bind:src="'http://openweathermap.org/img/w/' + weather[0].icon + '.png'" alt=""> <br>
                            <ul class="weather-con">
                                <li v-for="wea in weather">&nbsp;&nbsp;&nbsp;{{wea.main}}&nbsp;&nbsp;&nbsp;</li>
                            </ul>
                        </v-flex>
                        <v-flex xs6 d-flex align-center justify-center>
                            <span class="temp">{{temp.temp}}°C</span>
                        </v-flex>
                    </v-layout>
                </v-card>
            </v-flex>
        </v-layout>
    </v-container>
</template>
<script>
export default {
    name   : 'weatherApp',
    data   : () => ({
        valid   : true,
        city    : '',
        empty   : [
            v => !!v || 'Invalid city.',
            v => (v && v.length > 0) || 'Invalid city.'
        ],
        cityName: [],
        country : [],
        temp    : [],
        weather : []
    }),
    methods: {
        submit() {
            if (this.$refs.form.validate()) {
                axios.get('http://api.openweathermap.org/data/2.5/weather?q=' + this.city + '&units=metric&appid=4131d1a9222501fde97ae731c6dc49be')
                     .then((response) => {
                         console.log(response.data);
                         this.cityName = response.data.name;
                         this.country  = response.data.sys;
                         this.temp     = response.data.main;
                         this.weather  = response.data.weather;
                     })
                     .catch((error) => {
                         console.log(error);
                     });
            }
        },
        clear() {
            this.$refs.form.reset();
        }
    }
};
</script>
