<template>
    <div style="font-family: Arial, Helvetica, sans-serif;">
        <div class="column" style="position:relative;z-index:1">
            <div class="ui message red" v-show="error">{{error}}</div>
            <form class="inline">
                <div>
                    <input class="input" type="text" placeholder="Search..." v-model="location" @input="onChange" />
                    <div style="padding-top: 30px;">
                        <header class="total-results" v-show="isOpen">Results found: {{ totalResults }}</header>
                        <form v-show="isOpen" class="autocomplete-results">
                            <li v-for="(result, i) in results" :key="i" @click="setResult(result)">
                                <div class="autocomplete-result">
                                    <div v-text="result.name"></div>
                                    <div v-text="result.location.lat + ', ' + result.location.lon" class="autocomplete-location"></div>
                                </div>
                            </li>
                        </form>
                    </div>
                    <Modal v-show="isModalVisible" @close="closeModal" :data=details />
                </div>
            </form>
        </div>
        <section id="map"></section>
    </div>
</template>
  
<script>

import Modal from '../components/DetailsModal.vue';
import pinIcon from '../assets/icon-pin.svg';
import { sampleData } from '../assets/sample-data';


export default {

    data() {
        return {
            address: "",
            data: sampleData,
            error: "",
            isOpen: false,
            location: "",
            isModalVisible: false,
            results: [],
            totalResults: 0,
            details: {}
        }
    },

    mounted() {
        this.showLocationOnMap(-34.397, 150.644, true) // true is a parameter that represents initial startup
    },

    components: {
        Modal
    },

    methods: {
        // Filter results by name
        filterData() {
            this.results = this.data.filter(item => item.name.toLowerCase().indexOf(this.location.toLowerCase()) > -1);
        },
        // Watch for changes in the input text field
        onChange() {
            this.filterData();
            this.totalResults = this.results.length // Number of matching results
            this.isOpen = true; // Open options/autocomplete list
        },
        // Set result when matching result is selected
        setResult(result) {
            this.location = result.name;
            this.isOpen = false;
            this.showLocationOnMap(result.location.lat, result.location.lon, false);
            this.details = result; // details object is set to the selected result object
        },
        // Show the location of on Google map
        showLocationOnMap(lat, lon, init) {
            let map = new google.maps.Map(document.getElementById("map"), {
                center: {
                    lat: lat,
                    lng: lon
                },
                scrollwheel: false,
                zoom: 15,
                clickableIcons: false,
                disableDefaultUI: true,
            });

            // If this is initial call, do not set a marker on initial location
            if (!init) {
                let marker = new google.maps.Marker({
                    position: new google.maps.LatLng(lat, lon),
                    map: map,
                    icon: pinIcon
                });
                marker.addListener("click", () => {
                    this.isModalVisible = true;
                });
            }
            // close autocomplete list when user clicks outside of text field (the map)
            map.addListener("click", () => {
                this.isOpen = false;
            });
        },
        showModal() {
            this.isModalVisible = true;
        },
        closeModal() {
            this.isModalVisible = false;
        }
    }
};
</script>
  
<style scoped>
@import '../assets/styles/Map.css';
</style>