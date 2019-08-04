<template lang=pug>
    v-list.pa-0(style="background-color:#fafafa" reverse)
        template(v-for="hotDog in hotDogs")
            HotDogsListItem(:hotDog="hotDog")
            v-divider
</template>

<script>
    import HotDogsListItem from "./HotDogsListItem";

    export default {
        components: {HotDogsListItem},
        props: {
            hotDogs: Array
        },
    };
</script>
