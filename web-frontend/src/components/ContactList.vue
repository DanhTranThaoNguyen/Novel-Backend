<script>
export default {
  props: {
    contacts: { type: Array, default: [] },
    activeIndex: { type: Number, default: -1 },
  },
  emits: ["update:activeIndex"],
  methods: {
    updateActiveIndex(index) {
      this.$emit("update:activeIndex", index);
    },
  },
  methods: {
    goToReadnovel() {
      this.$router.push({ name: "contact.read" });
    },
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (error) {
        console.log(error);
        // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
        this.$router.push({
          name: "notfound",
          params: {
            pathMatch: this.$route.path.split("/").slice(1),
          },
          query: this.$route.query,
          hash: this.$route.hash,
        });
      }
    },
  },
  created() {
    this.getContact(this.id);
    this.message = "";
  },
};
</script>
<template>
  <ul class="row g-2">
    <div class="row g-2">
      <div
        class="card"
        style="width: 18rem"
        v-for="(contact, index) in contacts"
        :key="contact._id"
        :class="{ active: index === activeIndex }"
        @click="updateActiveIndex(index)"
      >
        <img src="" class="card-img-top" alt="..." />
        <div class="card-body"> 
          <h5>{{ contact.name }}</h5>
           <!-- <img src="web-frontd/img" alt="" srcset=""> -->
        </div>
        <router-link class="link"
            :to="{
              name: 'contact.read',
              params: { id: activeIndex._id },
            }"
          >
          <button class="btn btn-sm btn-success" @click="goToReadnovel">
          <i class="fa-solid fa-chart-simple"></i> doc truyen
        </button> 
          </router-link>
      </div>
      
    </div>
  </ul>
</template>
<style scoped>
@import "@/assets/list.css";
</style>
