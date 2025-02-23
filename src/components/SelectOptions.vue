<template>
    <div class="options">
        <div class="multi-select">
            <label class="typo__label">Simple select / dropdown</label>
            <MultiSelect id="multiselect" v-model="selectedOptions" :options="options" :multiple="true"
                :close-on-select="false" :clear-on-select="false" :preserve-search="true" placeholder="Pick some"
                label="name" track-by="name" :preselect-first="true">
                <template #selection="{ values, isOpen }">
                    <span class="multiselect__single" v-if="values.length" v-show="!isOpen">{{ values.length }} options
                        selected</span>
                </template>
            </MultiSelect>
            <div v-if="selectedOptions.length > 0" class="selected-option">
                <div v-for="option in selectedOptions" :key="option.name">
                    <input type="checkbox" :checked="true" disabled>{{ option.name }}
                </div>
            </div>
        </div>
        <div class="generate-vector">
            <label v-if="this.loading == true" class="status-label">Generating Vector Please Wait...</label>
            <label v-else-if="this.loading == false & status == 1" class="status-label">Vector is Generated</label>
            <button @click="generateVector" class="generate-button">Generate Vector</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import MultiSelect from 'vue-multiselect';

export default {
    components: { MultiSelect },
    data() {
        return {
            selectedOptions: [],
            options: [],
            status: 0,
            loading: false,
        };
    },
    created() {
        this.fetchOptions();
    },
    methods: {
        async fetchOptions() {
            try {
                const response = await axios.get(process.env.VUE_APP_PYTHON_UTILITY_SERVICE_URL+'get_categories');
                console.log("response", response.data);
                this.options = response.data;
            } catch (error) {
                console.error('Error fetching options:', error);
            }
        },
        async generateVector() {
            const allSelectedOptions = [];
            console.log('Selected options:', this.selectedOptions);
            console.log('All options:', this.options);
            this.selectedOptions.forEach(option => {
                console.log(option['name']);
                allSelectedOptions.push(option['name']);
            });
            this.loading = true;
            try {
                const response = await axios.post(process.env.VUE_APP_PYTHON_UTILITY_SERVICE_URL+'generate_vector', {
                    categories: this.options,
                    selectedOptions: allSelectedOptions
                }).then(async () => {
                    const resp = await axios.get(process.env.VUE_APP_PYTHON_UTILITY_SERVICE_URL+'check_status');
                    console.log("response from fetch status: ", resp.data);
                    this.status = resp.data.status;
                    this.loading = false;
                });
                console.log('Vector generated:', response.data);
                
            } catch (error) {
                console.error('Error generating vector:', error);
            }
        }
    }
};
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style scoped>
.options {
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.multi-select {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.typo__label {
    font-weight: bold;
    margin-bottom: 5px;
}

.selected-option {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 5px;
    background-color: #e0e0e0;
    border-radius: 4px;
}

.generate-vector {
    display: flex;
    flex-direction: column;
    gap: 10px;
    align-items: flex-start;
}

.status-label {
    color: green;
    font-weight: bold;
}

.generate-button {
    padding: 10px 20px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.generate-button:hover {
    background-color: #0056b3;
}
</style>