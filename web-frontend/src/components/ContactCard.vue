<script>
    export default {
        props: {
            contact: { type: Object, required: true }
        }
    }
</script>
<template>
    <div>
        <div class="p-1">
            <strong>tên:</strong>
            {{ contact.name }}
        </div>
        <div class="p-1">
            <strong>The loai:</strong>
            {{ contact.category }}
        </div>
        <div class="p-1">
            <strong>Tac gia:</strong>
            {{ contact.author }}
        </div>
        <div class="p-1">
            <strong>Mo ta:</strong>
            {{ contact.describe }}
        </div>
        <div class="p-1">
            <strong>Liên hệ yêu thích:&nbsp</strong>
            <i v-if="contact.favorite" class="fas fa-check"></i>
            <i v-else class="fas fa-times"></i>
        </div>
    </div>
</template>

            