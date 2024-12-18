<template>
    <div class="profile-wrapper">
        <button @click="goBack" class="back-button">Go back</button>
        <section class="profile-content">
            <header>
                <h2>{{ person.fullName }}</h2>
                <p>{{ person.email }}</p>
            </header>
            <figure>
                <img :src="person.avatar" alt="Avatar" class="avatar-img" />
            </figure>
        </section>
    </div>
</template>

<script setup>
import { ref, computed } from "vue";
import peopleData from "../peopledata";

const props = defineProps({
    id: {
        type: [String, Number],
        required: true,
    },
});

const person = computed(() => {
    return peopleData.find((p) => p.id == props.id) || {};
});

const fullName = computed(
    () => `${person.value.first_name} ${person.value.last_name}`
);

const goBack = () => window.history.back();
</script>

<style scoped>
body,
html {
    height: 100%;
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #2a2a2a;
    display: flex;
    justify-content: center;
    align-items: center;
}

.profile-wrapper {
    max-width: 500px;
    width: 100%;
    height: auto; /* Adjust height based on content */
    border-radius: 16px;
    padding: 20px;
    margin: 30px auto;
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
    color: #f4f4f4;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.go-back-btn {
    padding: 12px 20px;
    background-color: #ff5c8d;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
    margin-bottom: 20px;
    transition: background-color 0.3s ease;
    width: 100%;
    max-width: 200px;
    text-align: center;
}

.go-back-btn:hover {
    background-color: #ff4371;
}

.profile-header {
    text-align: center;
    margin-bottom: 20px;
}

.profile-header h2 {
    font-size: 26px;
    font-weight: 600;
    color: #fff;
    margin-bottom: 10px;
}

.profile-header p {
    color: #b1b1b1;
    font-size: 16px;
}

.avatar-img {
    width: 150px;
    height: 150px;
    object-fit: cover;
    border-radius: 50%;
    margin-top: 20px;
    border: 4px solid #fff;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
}
</style>
