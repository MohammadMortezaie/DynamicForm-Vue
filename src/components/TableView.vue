<template>
    <v-card>
        <v-card-title>
            <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="Search"
                single-line
                hide-details
            >
            </v-text-field>
        </v-card-title>
        <v-data-table :headers="head" :items="list" :search="search">
            <template v-slot:[`item.img`]="{ item }">
                <v-img
                    max-height="110"
                    max-width="110"
                    v-bind:src="item.img"
                ></v-img>
            </template>
        </v-data-table>
    </v-card>
</template>
<script>
export default {
    props: {
        head: Array,
        list: Array,
    },
    data() {
        return {
            search: "",
        };
    },
};
</script>