<template>
    <v-container fluid gird-list-xs>
        <v-layout row wrap>
            <v-flex xs12 class="mb-3">
                <h2>{{msg}}</h2>
            </v-flex>
            <v-flex xs12>
                <section>
                    <v-form ref="form" v-model="valid" lazy-validation required>
                        <v-text-field label="City, Country Code (eg. Toronto, CA)" prepend-icon="place" v-model="city" :rules="empty"></v-text-field>
                        <v-btn @click="submit">Search</v-btn>
                        <v-btn @click="clear">clear</v-btn>
                    </v-form>
                </section>
            </v-flex>
            <v-flex xs12>
                <section class="mt-5" v-if="cityName[0]">
                    <h2>
                        {{cityName}}, {{country.country}}
                    </h2>
                    <span>{{temp.temp}}°C</span><br>
                    <ul>
                        <li v-for="wea in weather">{{wea.main}}</li>
                    </ul>
                </section>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
export default {
    name   : 'weatherApp',
    props  : {
        msg: String
    },
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
