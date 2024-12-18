<template>
    <div class="filters">
        <button @click="sortTable('first_name')">
            <p :class="getArrowClass('first_name')">First name</p>
        </button>
        <button @click="sortTable('last_name')">
            <p :class="getArrowClass('last_name')">Last name</p>
        </button>
        <button @click="sortTable('id')">
            <p :class="getArrowClass('id')">ID</p>
        </button>
        <button @click="sortTable('email')">
            <p :class="getArrowClass('email')">Email</p>
        </button>
    </div>

    <div class="people-list">
        <PersonCard
            class="card"
            v-for="person in sortedPeople"
            :key="person.id"
            :person="person"
        />
    </div>
    <div id="scroll-test"></div>
</template>

<script setup>
import { ref, computed } from "vue";
import peopleData from "../peopledata";
import PersonCard from "@/components/PersonCard.vue";

const people = ref(peopleData);
const sortingBy = ref("");
const sortingDir = ref("asc");

const sortedPeople = computed(() => {
    return people.value.slice().sort((a, b) => {
        const aValue = a[sortingBy.value];
        const bValue = b[sortingBy.value];

        if (typeof aValue === "string" && typeof bValue === "string") {
            return sortingDir.value === "asc"
                ? aValue.localeCompare(bValue)
                : bValue.localeCompare(aValue);
        }

        if (typeof aValue === "number" && typeof bValue === "number") {
            return sortingDir.value === "asc"
                ? aValue - bValue
                : bValue - aValue;
        }

        return 0;
    });
});

const sortTable = (column) => {
    if (sortingBy.value === column) {
        sortingDir.value = sortingDir.value === "asc" ? "desc" : "asc";
    } else {
        sortingBy.value = column;
        sortingDir.value = "asc";
    }
};

const getArrowClass = (column) => {
    if (sortingBy.value === column) {
        return sortingDir.value === "asc" ? "arrow-up" : "arrow-down";
    }
    return "no-arrow";
};
</script>

<style scoped>
/* Filters Container */
.filters {
    margin: 20px 0;
    display: flex;
    gap: 15px;
    justify-content: center;
}

/* Filter Buttons */
.filters button {
    background: linear-gradient(135deg, #6e8efb, #a777e3);
    color: #fff;
    font-size: 16px;
    font-weight: bold;
    border: none;
    border-radius: 25px;
    padding: 12px 20px;
    cursor: pointer;
    transition: background 0.3s, transform 0.2s, box-shadow 0.3s;
    box-shadow: 0 4px 8px rgba(110, 142, 251, 0.4);
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 160px;
}

.filters button:hover {
    background: linear-gradient(135deg, #5a7efb, #935ed1);
    transform: translateY(-3px);
    box-shadow: 0 6px 12px rgba(110, 142, 251, 0.6);
}

/* Sorting Arrows */
.no-arrow::after,
.arrow-up::after,
.arrow-down::after {
    font-size: 18px;
    margin-left: 8px;
}

.no-arrow::after {
    content: "–";
    color: #b0b0b0;
}

.arrow-up::after {
    content: "▲";
    color: #fff;
}

.arrow-down::after {
    content: "▼";
    color: #fff;
}

/* People List */
.people-list {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
}

/* Card Styling */
.card {
    background: #1e1e2e;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    color: #e0e0e0;
    transition: transform 0.3s, box-shadow 0.3s;
    min-width: 280px;
    max-width: 400px;
    flex: 1;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.4);
}

/* Scroll Test Div */
#scroll-test {
    margin-top: 150vh;
    text-align: center;
    color: #b0b0b0;
    font-style: italic;
}
</style>
