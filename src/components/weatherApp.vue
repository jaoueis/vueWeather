<template>
    <v-container fluid gird-list-xs>
        <v-layout row wrap>
            <v-flex xs12 class="mb-3">
                <h2>{{msg}}</h2>
            </v-flex>
            <v-flex xs12>
                <section>
                    <v-form ref="form" v-model="valid" lazy-validation required>
                        <v-text-field label="City, Country" prepend-icon="place" v-model="city" :rules="empty"></v-text-field>
                        <v-btn @click="submit">Search</v-btn>
                        <v-btn @click="clear">clear</v-btn>
                    </v-form>
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
        valid: true,
        city : '',
        empty: [
            v => !!v || 'Invalid city.',
            v => (v && v.length > 0) || 'Invalid city.'
        ]
    }),
    methods: {
        submit() {
            //console.log('api.openweathermap.org/data/2.5/weather?q=' + this.city);
            let city = this.city;
            if (this.$refs.form.validate()) {
                axios.get('http://api.openweathermap.org/data/2.5/weather?q=' + city + '&appid=4131d1a9222501fde97ae731c6dc49be')
                     .then(function (response) {
                         console.log(JSON.stringify(response));
                     })
                     .catch(function (error) {
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
