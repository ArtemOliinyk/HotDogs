<template lang="pug">
    v-list-item
        v-list-item-content
            v-list-item-title {{ hotDog.title }}
        v-list-item-action.ma-0
            v-layout.row
                v-btn.ma-1(icon small color="blue darken-1" dark @click="editHotDog")
                    v-icon edit
                v-btn.ma-1(icon small color="red darken-1" dark @click="deleteHotDog")
                    v-icon close
</template>

<script>
    import axios from "axios";

    export default {
        name: "HotDogsListItem",
        props: {
            hotDog: Object,
            index: Number,
            id: String
        },
        inject: ['update'],
        methods: {
            async editHotDog() {
                let title = "Updated hot dog";
                let url = 'http://localhost:3000/api/update';
                try {
                    await axios.put(url, {title: title, id: this.id});
                    this.update();
                } catch (e) {
                    console.log(1, e);
                }
            },
            async deleteHotDog() {
                let url = 'http://localhost:3000/api/delete';
                try {
                    await axios.delete(url, {data: {id: this.id}});
                    this.update();
                } catch (e) {
                    console.log(1, e);
                }
            }
        }
    }
</script>

<style scoped>

</style>