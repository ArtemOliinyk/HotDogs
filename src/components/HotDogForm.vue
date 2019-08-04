<template lang="pug">
    v-form(v-if="formDialog" v-model="isFormValid")
        v-dialog(v-model="formDialog" persistent max-width="600px")
            v-card
                v-card-title
                    span.headline {{title}}
                v-card-text
                    v-text-field(v-model.trim="hotDog.title"
                        :rules="rules.titleRules" label="Title*"
                        :counter="30" maxlength="30"
                        @keydown.enter.prevent="action")
                    small *indicates required field
                v-card-actions
                    v-spacer
                    v-btn(@click="close" color="blue darken-1") Close
                    v-btn(@click="action" color="blue darken-1" :disabled="!isFormValid") Save
</template>

<script>
    import axios from "axios";

    export default {
        name: "HotDogForm",
        data: () => ({
            isFormValid: false,
            newHotDog: {
                title: " "
            }
        }),
        inject: ["hotDogAction", "update"],
        props: {
            hotDog: {
                type: Object
            },
            formMode: {
                type: String
            },
            formDialog: {
                type: Boolean
            }
        },
        computed: {
            title() {
                if (this.formMode === "adding"){
                    return "Add hot dog";
                }
                else return "Edit hot dog"
            },
            action() {
                if (this.formMode === "adding") {
                    return this.addTodo;
                }
                else return this.editTodo;
            },
            rules() {
                return {
                    titleRules: [
                        v => !!v || 'Hod dog title is required',
                        v => !!v.trim() || 'Hod dog title must be valid',
                    ]
                }
            }
        },
        methods: {
            async addTodo() {
                let url = 'https://hot-dogs-ao.herokuapp.com/api/create';
                this.hotDog.title += ' hot dog';
                try {
                    await axios.post(url, this.hotDog);
                    this.hotDog.title = "";
                    this.hotDogAction(this.newHotDog, null, false);
                    this.update();
                } catch (e) {
                    alert(e.message);
                }

            },
            async editTodo() {
                let url = 'https://hot-dogs-ao.herokuapp.com/api/update';
                try {
                    await axios.put(url, this.hotDog);
                    this.hotDogAction(this.newHotDog, null, false);
                    this.update();
                } catch (e) {
                    alert(e.message);
                }

            },
            close() {
                this.hotDogAction(this.newHotDog, null, false);
            }
        }
    }
</script>

<style scoped>

</style>