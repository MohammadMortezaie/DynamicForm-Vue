<template>
    <v-menu v-model="menu1" :close-on-content-click="false" max-width="290">
        <template v-slot:activator="{ on, attrs }">
            <v-text-field :value="computedDateFormattedMomentjs" clearable label="Select A Day" readonly v-bind="attrs"
                v-on="on" @click:clear="date = null"></v-text-field>
        </template>
        <v-date-picker v-model="date" @change="change"></v-date-picker>
    </v-menu>
</template>

<script>
import VDatePicker from "vuetify/lib/components/VDatePicker/VDatePicker";
import moment from "moment";
import { bus } from "@/main";

export default {
    props: {

    },
    data() {
        return {
            menu1: false,
            date: '',
        };
    },
    components: {
        VDatePicker,
    },
    methods: {
        change() {
            this.menu1 = false
            bus.$emit("dateChange", this.date);
        }
    },
    computed: {
        computedDateFormattedMomentjs() {
            return this.date ? moment(this.date).format("dddd, MMMM Do YYYY") : "";
        },
    },
};

</script>
