<template>
    <div>
        <v-select :items="countries" v-model="selectedCountry" @change="getCities" label="Select Country"
            item-text="country" item-value="country"></v-select>

        <v-select :items="cities" v-model="selectedCity" :disabled="!countryChange" @change="setCity"
            label="Select City" item-text="state" item-value="state"></v-select>
    </div>
</template>

<script>
import country from "@/services/country.json";
import city from "@/services/city.json";
import VSelect from "vuetify/lib/components/VSelect";
import { bus } from "@/main";

export default {
    data() {
        return {
            countries: country,
            cities: [],
            countryChange: false,
            selectedCountry: "select country",
            selectedCity: "",
        };
    },
    components: {
        VSelect
    },
    methods: {
        getCities() {
            bus.$emit("countryChange", this.selectedCountry);
            this.cities = [];
            this.countryChange = true;
            city.forEach((item) => {
                if (item.country == this.selectedCountry) {
                    this.cities.push(item);
                }
            });
        },
        setCity() {
            bus.$emit("cityChange", this.selectedCity);

        }
    },
};
</script>
