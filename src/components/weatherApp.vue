<template>
    <v-container fluid gird-list-xs>
        <v-layout row wrap>
            <v-flex xs12 class="mb-3">
                <h1>Vue Weather App</h1>
            </v-flex>
            <v-flex xs12 class="mb-3">
                <h2>Please enter a city name in the text field.</h2>
            </v-flex>
            <v-flex xs12>
                <v-form>
                    <v-text-field label="City, Country" prepend-icon="place"></v-text-field>
                    <v-btn>Search</v-btn>
                </v-form>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
export default {
    name : 'weatherApp',
    props: {
        msg: String
    }
};
</script>
