<template>
    <div class="modal-backdrop" @click="close">
        <div class="modal">
            <header class="modal-header">
                <slot name="header">
                    <form class="data">
                        <img class="icon" src="../assets/icon-pin.svg" />
                        <div>
                            <div v-if="data.name" v-text="data.name" class="name"></div>
                            <div v-if="data.location" v-text="data.location.lat + ', ' + data.location.lon" class="location"></div>
                        </div>
                    </form>
                </slot>
                
                <button v-if="data.details && data.details.website" class="web-btn" type="button" @click="openWebsite(data.details.website)">
                    Visit Website
                </button>
            </header>

            <section  class="modal-body">
                <slot name="body">
                    <div v-if="data.details && data.details.description" v-text="data.details.description" class="data description"></div>
                    <div v-if="data.images" class="container" style="list-style: none;">
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

    methods: {
        close() {
            this.$emit('close');
        },
        // Opens website of location in a new window
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