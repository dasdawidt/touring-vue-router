<template>
    <p>Register for the event here</p>
    <button @click="register">Register me</button>
</template>

<script>
export default {
    props: ['event'],
    inject: ['GStore'],
    methods: {
        register() {
            // Call to API
            // If registered then redirect to event details

            this.GStore.flashMessage = `You are successfully registered for ${this.event.title}.`
            setTimeout(() => {
                this.GStore.flashMessage = ''
            }, 3000)

            this.$router.push({
                name: 'EventDetails',
                // Specifying id is obsolete because we redirect to parent component
                // params: { id: this.event.id },
            })
        },
    },
}
</script>
