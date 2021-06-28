<template>
    <div class="text-center">
        
        <h2 class="text-3xl font-bold"><span :class=concatCountry></span> {{text}}</h2>
        <div class="text-2xl mt-4 mb-10">{{timestamp}}</div>
    </div>
</template>

<script>
import moment from 'moment'
import 'flag-icon-css/css/flag-icon.css'

export default {
    name: 'DateTitle',
    props: ['text', 'dataDate', 'countryCode'],

    computed: {
        timestamp: function() {
            return moment(this.dataDate).format('MMMM Do YYYY, h:mm:ss a')
        },
        concatCountry: function() {
            return 'flag-icon flag-icon-' + this.countryCode;
        }
    },
}
</script>