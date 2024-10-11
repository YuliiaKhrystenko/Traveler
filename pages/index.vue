<template>
    <div class="min-h-screen bg-gray-100 p-8">
        <div class="grid grid-cols-2 gap-16">
            <!-- Country Search -->
            <div class="mb-8">
                <h2 class="text-xl text-gray-600 font-semibold mb-4">Countries List</h2>
                <input 
                    v-model="searchQuery" 
                    placeholder="Search for a country"
                    class="border border-gray-300 p-2 rounded-lg w-full">
                <ul class="mt-4">
                    <li 
                        v-for="country in filteredCountries" 
                        :key="country.countryCode"
                        class="p-4 mb-2 bg-white shadow hover:bg-gray-200 transition cursor-pointer"
                        
                    >
                        <NuxtLink :to="`/country/${country.countryCode}`" class="text-blue-600 hover:underline">
                             {{ country.name }}
                         </NuxtLink>
                    </li>
                </ul>
            </div>

            <!-- Random Countries Widget -->
            <div class="flex flex-col">
                <h2 class="text-xl text-gray-600 font-semibold mb-4">Random Countries Widget</h2>
                <div class="bg-white p-6 rounded-lg shadow-lg">
                    <div 
                        v-for="holiday in randomHolidays" 
                        :key="holiday.date" class="mb-4">
                        <h3 class="font-semibold">
                            {{ holiday.countryCode }}
                        </h3>
                        <p>{{ holiday.localName }}</p>
                        <p>{{ holiday.date }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script lang="ts" setup>
import { ref, watch, onMounted } from 'vue'

interface Country {
    countryCode: string;
    name: string;     
}

interface Holiday {
    countryCode: string;
    localName: string;
    date: string;
}

interface HolidayResponse {
    countryCode: string;
    localName: string;
    date: string;
    counties: string | null;
    fixed: boolean;
    global: boolean;
    launchYear: number | null;
    name: string;
    types: string[];
}



const countries = ref<Country[]>([])
const filteredCountries = ref<Country[]>([])
const randomHolidays = ref<Holiday[]>([])
const searchQuery = ref<string>('');



const fetchCountries = async (): Promise<void> => {
    const response = await fetch('https://date.nager.at/api/v3/AvailableCountries')
    const data = await response.json();
    console.log('Fetched countries:', data); 
    countries.value = data
    filteredCountries.value = data
}

const fetchRandomHolidays = async (): Promise<void> => {
    const response = await fetch('https://date.nager.at/api/v3/NextPublicHolidaysWorldwide');
    const holidays: HolidayResponse[] = await response.json();

    randomHolidays.value = holidays.slice(0, 3).map((holiday: HolidayResponse) => ({
        countryCode: holiday.countryCode,
        localName: holiday.localName,
        date: holiday.date,
    }));
}

watch(searchQuery, (newQuery) => {
    if (newQuery) {
        filteredCountries.value = countries.value.filter((country) =>
            (country.name || '').toLowerCase().includes(newQuery.toLowerCase())
        );
    } else {
        filteredCountries.value = countries.value;
    }
});

onMounted((): void => {
    fetchCountries()
    fetchRandomHolidays()
})

</script>
