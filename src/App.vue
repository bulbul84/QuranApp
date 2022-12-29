<script>
import axios from "axios";
export default {
    name: "App",
    data() {
        return {
            surah: [],
            currentSurah: [],
            loading: true,
        };
    },
    mounted() {
        axios.get("https://api.alquran.cloud/v1/surah").then((response) => {
            this.surah = response.data.data;
        });
        this.querySpecificSurah(1);
    },
    methods: {
        getSpecificSurah(e) {
            this.querySpecificSurah(e.target.value);
        },
        querySpecificSurah(surahNumber) {
            this.loading = true;
            axios.get("https://api.alquran.cloud/v1/surah/" + surahNumber).then((response) => {
                this.currentSurah = response.data.data;
                this.loading = false;
            });
        },
    },
};
</script>

<template>
    <div class="container mx-auto">
        <!-- <h2 class="mb-3 text-center text-3xl font-bold text-green-400">Quran App</h2> -->
        <!-- <input v-model="heading" type="text" /> -->

        <div class="quran-content rounded border p-5 py-5 shadow-lg">
            <div class="quran-header mb-5 grid grid-cols-3 gap-3 text-center">
                <div class="surah-select">
                    <select @change="getSpecificSurah" class="rounded border px-4 py-3" name="" id="">
                        <option v-for="sura in surah" :value="sura.number">
                            {{ sura.name }} - {{ sura.englishName }}
                        </option>
                    </select>
                </div>
                <div class="surah-name">
                    <h2 class="text-xl font-bold">{{ currentSurah.englishName }}</h2>
                    <p>{{ currentSurah.englishNameTranslation }}</p>
                    <h4>Ayah: {{ currentSurah.numberOfAyahs }}</h4>
                </div>
                <!-- <div class="surah-ayah text-right">
                    <select class="rounded border px-4 py-3" name="" id="">
                        <option value="">Select Ayah</option>
                    </select>
                </div> -->
            </div>
            <div class="quran-body">
                <div v-if="loading" class="flex items-center justify-center gap-2 font-semibold">
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke-width="1.5"
                        stroke="currentColor"
                        class="h-6 w-6 animate-spin">
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0l3.181 3.183a8.25 8.25 0 0013.803-3.7M4.031 9.865a8.25 8.25 0 0113.803-3.7l3.181 3.182m0-4.991v4.99" />
                    </svg>
                    Loading...
                </div>

                <div v-else class="surah-full text-xl" dir="rtl">
                    <ul>
                        <li
                            class="mb-3 flex gap-2"
                            v-if="currentSurah.hasOwnProperty('ayahs')"
                            v-for="ayah in currentSurah.ayahs">
                            <span class="font-bold">{{ ayah.numberInSurah }}</span> -
                            <span>{{ ayah.text }}</span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.quran-content {
    max-width: 850px;
    width: 100%;
    margin: 0px auto;
}
</style>
