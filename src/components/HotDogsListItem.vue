<template lang="pug">
    v-list-item
        v-list-item-content
            v-list-item-title {{ hotDog.title }}
        v-list-item-action.ma-0
            v-layout.row
                v-btn.ma-1(icon small color="blue darken-1" dark @click="editHotDog")
                    v-icon mdi-pencil
                v-btn.ma-1(icon small color="red darken-1" dark @click="deleteHotDog")
                    v-icon mdi-close
</template>

<script>
    import axios from "axios";

    export default {
        name: "HotDogsListItem",
        props: {
            hotDog: Object,
        },
        inject: ["hotDogAction", "update"],
        data: () => ({
            formDialog: false,
            formMode: "editing",
        }),
        methods: {
            async editHotDog() {
                this.formDialog = true;
                this.hotDogAction(this.hotDog, this.formMode, this.formDialog)
            },
            async deleteHotDog() {
                let url = 'https://hot-dogs-ao.herokuapp.com/api/delete';
                try {
                    await axios.delete(url, {data: {_id: this.hotDog._id}});
                    this.update();
                } catch (e) {
                    alert(e.message);
                }
            }
        }
    }
</script>

<style scoped>

</style>