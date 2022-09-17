<template>
    <div class="modal-backdrop" @click="close">
        <div class="modal">
            <header class="modal-header">
                <slot name="header">
                    <form class="data">
                        <img class="icon" src="../assets/icon-pin.svg" />
                        <div>
                            <div class="name">{{ data.name }}</div>
                            <div class="location">{{ data.location.lat }}, {{ data.location.lon }}</div>
                        </div>
                    </form>

                </slot>
                <button class="web-btn" type="button" @click="openWebsite(data.details.website)">Visit
                    Website</button>
            </header>

            <section class="modal-body">
                <slot name="body">
                    <div class="data">{{ data.details.description }}</div>
                    <div class="container" style="list-style: none;">
                        <li class="column" v-for="img in data.images">
                            <img class="images" :src=img />
                        </li>
                    </div>
                </slot>
            </section>
        </div>
    </div>
</template>

<script>


export default {
    name: 'Modal',

    mounted() {
        
    },

    methods: {
        close() {
            this.$emit('close');
        },
        openWebsite(website) {
            window.open(website);
        },
    },
    props: {
        data: Object
    },
};
</script>

<style>
    @import '../assets/styles/DetailsModal.css';
</style>