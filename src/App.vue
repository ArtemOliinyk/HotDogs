<template lang="pug">
    div#app
        v-app
            p.text-center.display-1 Hot Dogs
            div.text-right.ma-2
                v-btn(@click="add" color="blue")
                    | New hot dog
            HotDogsList(:hotDogs="hotDogs")

</template>

<script>

    import HotDogsList from "./components/HotDogsList";
    import axios from "axios";

    export default {
        name: 'App',
        components: {
            HotDogsList
        },
        provide() {
            return {
                update: () => this.getHotDogs()
            }
        },
        data: () => ({
            hotDogs: [],
            index: 0
        }),
        methods: {
            async add() {
                let url = 'http://localhost:3000/api/create';
                try {
                    await axios.post(url, {title: `New hot dog ${this.index}`});
                    this.getHotDogs();
                    this.index++;
                } catch (e) {
                    console.log(2, e.message);
                }

            },
            async getHotDogs() {
                let url = 'http://localhost:3000/api/getAll';
                try {
                    let response = await axios.get(url);
                    this.hotDogs = response.data.reverse();
                } catch (e) {
                    console.log(1, e.message);
                }
            },
        },
        created() {
            this.getHotDogs();
        },
    };
</script>
<style>
    #app {
    }
</style>
