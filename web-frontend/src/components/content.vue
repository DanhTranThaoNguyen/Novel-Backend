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
            <strong>Chap:</strong>
            {{ contact.chaper }}
        </div>
        <div class="p-1">
            <strong>Noi dung:</strong>
            {{ contact.content }}
        </div>
    </div>
</template>

            