<template>
    <h1>Events for Good</h1>
    <div class="events">
        <EventCard v-for="event in events" :key="event.id" :event="event" />

        <div class="pagination">
            <router-link
                id="page-prev"
                :to="{ name: 'EventList', query: { page: page - 1 } }"
                rel="prev"
                :disabled="page === 1"
                v-if="page !== 1"
            >
                &#10140;
            </router-link>
            <p id="page-number">{{ page }} / {{ totalPages }}</p>
            <router-link
                id="page-next"
                :to="{ name: 'EventList', query: { page: page + 1 } }"
                rel="next"
                :disabled="!hasNextPage"
                v-if="hasNextPage"
            >
                &#10140;
            </router-link>
        </div>
    </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { watchEffect } from 'vue'

export default {
    name: 'EventList',
    props: {
        page: {
            type: Number,
        },
    },
    components: {
        EventCard,
    },
    data() {
        return {
            events: null,
            totalEvents: 0,
        }
    },
    created() {
        watchEffect(() => {
            this.events = null
            EventService.getEvents(2, this.page)
                .then((response) => {
                    this.events = response.data
                    this.totalEvents = response.headers['x-total-count']
                })
                .catch((error) => {
                    console.log(error)
                })
        })
    },
    computed: {
        hasNextPage() {
            return this.page < this.totalPages
        },
        totalPages() {
            return Math.ceil(this.totalEvents / 2)
        },
    },
}
</script>

<style scoped>
.events {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.pagination {
    display: flex;
    width: 290px;
    /* Custom: */
    justify-content: space-between;
    align-items: baseline;
}

.pagination a {
    flex-grow: 0;
    text-decoration: none;
    color: #2c3e50;
    /* Custom: */
    padding: 0.25rem 0.75rem 0.3rem 0.75rem;
    border: 1px solid #2c3e50;
    border-radius: 0.25rem;
}

/* Custom: */
.pagination a:hover {
    opacity: 0.8;
}

#page-prev {
    text-align: left;
    /* Custom: */
    transform: scaleX(-1);
}

#page-next {
    text-align: right;
    /* Custom: */
    margin-left: auto;
}

#page-number {
    flex-grow: 1;
    text-align: center;
}
</style>
