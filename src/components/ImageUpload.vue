<template>
    <v-file-input
        v-model="imgFile"
        :rules="rules"
        accept="image/*"
        placeholder="Upload Image"
        prepend-icon="mdi-camera"
        label="Upload Image"
        color="#bada55"
        required
        @change="uploadImage()"
    >
    </v-file-input>
</template>

<script>
import { bus } from "@/main";
export default {
    data() {
        return {
            imgFile: null,
            rules: [
                (value) =>
                    !value ||
                    value.size < 2000000 ||
                    "Avatar size should be less than 2 MB!",
            ],
        };
    },
    methods: {
        uploadImage() {
            const reader = new FileReader();
            let base64Img = "";
            reader.onload = (event) => {
                base64Img = event.target.result;
                bus.$emit("image", base64Img);
            };
            reader.readAsDataURL(this.imgFile);
        },
    },
};
</script>
