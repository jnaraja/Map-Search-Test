<template>
    <div>
        <section class="ui three column left grid" style="position:relative;z-index:1">
            <div class="column">
                <div class="ui message red" v-show="error">{{error}}</div>
                <form class="inline">
                    <div class="input-icons">
                        <img class="icon" src="../assets/icon-search.svg">
                        <input class="input" type="text" placeholder="Search..." v-model="location" @input="onChange" />
                        <ul v-show="isOpen" class="autocomplete-results">
                            <li v-for="(result, i) in results" :key="i" @click="setResult(result)">
                                <form>
                                    <img class="icon" src="../assets/icon-pin.svg">
                                    <div class="autocomplete-result">
                                        <div>{{ result.name }}</div>
                                        <div class="autocomplete-location">
                                            {{ result.location.lat }},
                                            {{result.location.lon}}
                                        </div>
                                    </div>

                                </form>
                            </li>
                        </ul>
                        <Modal v-show="isModalVisible" @close="closeModal" :data=details />
                    </div>
                </form>
            </div>

        </section>
        <section id="map"></section>

    </div>
</template>
  
<script>

import axios from 'axios';
import Modal from '../components/DetailsModal.vue'
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
        this.showLocationOnMap(-34.397, 150.644, true)
    },

    components: {
        Modal
    },

    methods: {
        // TODO: get method that will retrive lat, lon from sample-data
        filterData() {
            this.results = this.data.filter(item => item.name.toLowerCase().indexOf(this.location.toLowerCase()) > -1);
        },
        onChange() {
            this.filterData();
            this.totalResults = this.results.length
            this.isOpen = true;
        },
        setResult(result) {
            this.location = result.name;
            this.isOpen = false;
            this.showLocationOnMap(result.location.lat, result.location.lon, false);
            this.details = result;
        },
        handleClickOutside(event) {
            if (!this.$el.contains(event.target)) {
                this.isOpen = false;
            }
        },
        getAddressFrom(lat, lon) {
            axios.get("https://maps.googleapis.com/maps/api/geocode/json?latlng="
                + lat +
                ","
                + lon
                + "&key=AIzaSyATLqfqG-AiTgWPFlPjDFRRL5vRIRFbZnk")
                .then(response => {
                    if (response.data.error_message) {
                        this.error = response.data.error_message;
                        // console.log(response.data.error_message);
                    } else {
                        this.address = response.data.results[0].formatted_address
                        // console.log(response.data.results[0].formatted_address);
                    }
                })
                .catch(error => {
                    this.error = error.message;
                    // console.log(error.message);
                })
        },
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

            if (!init) {
                let marker = new google.maps.Marker({
                    position: new google.maps.LatLng(lat, lon),
                    map: map,
                    icon: pinIcon
                });
                marker.addListener("click", () => {
                    console.log(this.details);
                    this.isModalVisible = true;
                });
            }
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