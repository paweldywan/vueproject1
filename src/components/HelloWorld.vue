<template>
    <div class="post">
        <div v-if="data.loading" class="loading">
            Loading... Please refresh once the ASP.NET backend has started. See <a href="https://aka.ms/jspsintegrationvue">https://aka.ms/jspsintegrationvue</a> for more details.
        </div>

        <div v-if="data.post" class="content">
            <table>
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Temp. (C)</th>
                        <th>Temp. (F)</th>
                        <th>Summary</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="forecast in data.post" :key="forecast.date">
                        <td>{{ forecast.date }}</td>
                        <td>{{ forecast.temperatureC }}</td>
                        <td>{{ forecast.temperatureF }}</td>
                        <td>{{ forecast.summary }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script lang="ts" setup>
    import { reactive, onMounted } from 'vue';

    type Forecasts = {
        date: string
    }[];

    interface Data {
        post: null | Forecasts,
        loading: boolean
    }

    const data = reactive<Data>({
        loading: true,
        post: null
    });

    const fetchData = async () => {
        data.post = null;

        data.loading = true;

        const r = await fetch('weatherforecast');

        const json = await r.json();

        data.post = json as Forecasts;

        data.loading = false;
    };

    onMounted(fetchData);
</script>