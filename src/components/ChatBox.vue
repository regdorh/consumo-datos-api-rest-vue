<template>
    <div id="box" class="container mb-3">
        <div v-for="(message, index) in messages" :key="index" class="d-flex flex-column"
            :class="message.userId == firstUser.id.value ? 'align-items-start' : message.userId == secondUser.id.value ? 'align-items-end' : ''">
            <small class="time fw-light text-secondary mb-1">{{ message?.name }} {{ message.last }}</small>
            <div class="d-flex align-items-center">
                <small class="time text-secondary"
                    :class="message.userId == firstUser.id.value ? 'align-items-start order-1' : message.userId == secondUser.id.value ? 'align-items-end order-2' : ''">{{
                        currentTime }}</small>
                <p class="my-0 px-3 rounded-4"
                    :class="message.userId == firstUser.id.value ? 'firstUserStyle order-2 ms-2' : message.userId == secondUser.id.value ? 'secondUserStyle order-1 me-2' : ''" :style="{backgroundColor: message.color}">
                    {{ message.message }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ChatBox',
    props: {
        messages: Array,
        firstUser: Object,
        secondUser: Object
    },
    data: function () {
        return {}
    },
    computed: {
        currentTime() {
            let date = new Date()
            let time = date.toLocaleTimeString('es-CL', { hour: '2-digit', minute: '2-digit', hour12: false });
            return time
        }
    },
}
</script>

<style scoped></style>