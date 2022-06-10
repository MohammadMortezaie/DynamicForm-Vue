<template>
    <v-card class="mx-auto px-4 my-4" elevation="2">
        <validation-observer ref="observer" v-slot="{ invalid }">
            <form @submit.prevent="newRow">
                <validation-provider
                    v-for="(field, index) in fields"
                    :key="index"
                    v-slot="{ errors }"
                    :name="field.name"
                    :rules="field.rules"
                >
                    <component
                        :is="field.type"
                        v-bind="field.props"
                        :name="field.name"
                        :error-messages="errors"
                        v-model="form.fields[field.name]"
                    />
                </validation-provider>

                <v-btn
                    class="mb-4"
                    :disabled="invalid"
                    type="submit"
                    color="#bada55"
                    >Save</v-btn
                >
            </form>
        </validation-observer>
    </v-card>
</template>
<script>
import { bus } from "../main";
import VTextarea from "vuetify/lib/components/VTextarea";
import VBtn from "vuetify/lib/components/VBtn";
import VSelect from "vuetify/lib/components/VSelect";
import VTextField from "vuetify/lib/components/VTextField/VTextField";
import VFileInput from "vuetify/lib/components/VFileInput/VFileInput";
import VDatePicker from "./DatePicker.vue";
import VSelectCountryCity from "./SelectCountyCity.vue";
import VImageUpload from "./ImageUpload.vue";

import {
    ValidationProvider,
    ValidationObserver,
    extend,
    setInteractionMode,
} from "vee-validate";
import { required } from "vee-validate/dist/rules";

export default {
    name: "FormView",
    props: {
        fields: Array,
    },
    data() {
        return {
            form: {
                fields: [],
            },
        };
    },
    components: {
        VTextarea,
        VSelect,
        VBtn,
        VTextField,
        ValidationProvider,
        ValidationObserver,
        VFileInput,
        VDatePicker,
        VSelectCountryCity,
        VImageUpload,
    },
    methods: {
        newRow() {
            this.$refs.observer.validate();
            bus.$emit("newRow", this.form.fields);
        },
    },
    mounted() {
        bus.$on("dateChange", (newDate) => {
            this.form.fields.date = newDate;
        });
        bus.$on("countryChange", (countryChange) => {
            this.form.fields.country = countryChange;
        });
        bus.$on("cityChange", (cityChange) => {
            this.form.fields.city = cityChange;
        });
        bus.$on("image", (img) => {
            this.form.fields.img = img;
        });
    },
};

setInteractionMode("eager");
extend("required", {
    ...required,
    message: "{_field_} can not be empty",
});
</script>
