<template>
    <div class="row ma-4">
        <v-scroll-x-transition appear>
            <FormView :fields="fields" />
        </v-scroll-x-transition>
        <TableView :list="listTable" :head="headTable" />
    </div>
</template>

<script>
import { bus } from "../main";

export default {
    data() {
        return {
            fields: [
                {
                    type: "v-text-field",
                    name: "fname",
                    rules: "required",
                    props: { color: "#bada55", label: "First Name" },
                },
                {
                    type: "v-text-field",
                    name: "lname",
                    rules: "required",
                    props: { color: "#bada55", label: "Last Name" },
                },
                {
                    type: "v-date-picker",
                    name: "date",
                },
                {
                    type: "v-select-country-city",
                    name: "country",
                },
                {
                    type: "v-textarea",
                    name: "desc",
                    rules: "required",
                    props: { color: "#bada55", label: "Description" },
                },
                {
                    type: "v-image-upload",
                },
            ],
            headTable: [
                {
                    text: "Image",
                    align: "start",
                    filterable: false,
                    value: "img",
                },
                { text: "Full Name", value: "fname" },
                { text: "Born", value: "born" },
                { text: "Day", value: "day" },
                { text: "Description", value: "description" },
            ],
            listTable: [],
        };
    },
    components: {
        FormView: () => import("@/components/FormView.vue"),
        TableView: () => import("@/components/TableView.vue"),
    },
    mounted() {
        bus.$on("newRow", (newData) => {
            const newRowTable = {
                img: newData.img,
                fname: newData.fname + " " + newData.lname,
                born: newData.country + " " + newData.city,
                day: newData.date,
                description: newData.desc,
            };
            this.listTable.push(newRowTable);
        });
    },
};
</script>
