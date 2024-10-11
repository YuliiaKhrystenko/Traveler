<template>
    <div class="p-8">
        <div class="flex items-center justify-between mb-4">
            <h1 class="text-xl text-gray-600 font-semibold mb-4">{{ country?.name.common }}</h1>
            <NuxtLink to="/" class="mb-4 px-4 py-2 bg-gray-300 text-black rounded-md hover:bg-gray-400">
                Home Page
            </NuxtLink>
        </div>

        <!-- Holiday List -->
        <div v-if="holidays.length > 0">
            <ul>
                <li 
                    v-for="holiday in holidays" 
                    :key="holiday.date"
                    class="p-4 mb-4 bg-white shadow"
                >
                    <h3 class="font-semibold">{{ holiday.localName }}</h3>
                    <p>{{ holiday.date }}</p>
                </li>
            </ul>
        </div>
        <div v-else>
            <p>No holidays available for {{ selectedYear }}</p>
        </div>

        <div class="flex justify-center items-center my-8">
            <button 
                v-for="year in years" 
                :key="year"
                :class="{ 'bg-blue-500 text-white': selectedYear === year, 'bg-gray-200': selectedYear !== year }"
                class="px-4 py-2 rounded-md mr-2" @click="fetchHolidays(year)">
                {{ year }}
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const countryCode = route.params.code;

const country = ref(null);
const holidays = ref([]);
const selectedYear = ref(new Date().getFullYear());
const years = ref(Array.from({ length: 11 }, (_, i) => 2020 + i)); 

const fetchCountryDetails = async () => {
    if (countryCode) {
        const response = await fetch(`https://restcountries.com/v3/alpha/${countryCode}`);
        const data = await response.json();
        console.log(data);
        
        country.value = data[0];
    } else {
        console.error('Country code is undefined.');
    }
};


const fetchHolidays = async (year = selectedYear.value) => {
    if (!countryCode) return;

    const response = await fetch(`https://date.nager.at/api/v3/PublicHolidays/${year}/${countryCode}`);
    const holidaysData = await response.json();

    holidays.value = holidaysData.map(holiday => ({
        localName: holiday.localName,
        date: holiday.date,
        types: holiday.types,
    }));

    selectedYear.value = year;
};

onMounted(() => {
    fetchCountryDetails();
    fetchHolidays(); 
});
</script>

