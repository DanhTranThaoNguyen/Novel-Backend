<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh Liên hệ</h4>
    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
    />
    <p>{{ message }}</p>
    <div class="page">
      <h4>Ngoi thieu</h4>
      <div class="mt-3 col-md-6">
        <div v-if="activeContact">
          <ContactCard :contact="activeContact" />
          <router-link
            :to="{
              name: 'contact.edit',
              params: { id: activeContact._id },
            }"
          >
            <span class="mt-2 badge badge-warning">
              <i class="fas fa-edit"></i> Hiệu chỉnh</span
            >
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import ContactCard from "@/components/ContactCard.vue";
import ContactService from "@/services/contact.service";

export default {
  components: {
    ContactCard,
  },
  props: {
    id: {type: String, required: true},
  },
  // Đoạn mã xử lý đầy đủ sẽ trình bày bên dưới
  data() {
    return {
      contacts: [],
      activeIndex: -1,
      message: "",
    };
  },
  computed: {
    // Chuyển các đối tượng contact thành chuỗi để tiện cho tìm kiếm.
    contactStrings() {
      return this.contacts.map((contact) => {
        const { name, category, author, describe } = contact;
        return [name, category, author, describe].join("");
      });
    },
    activeContact() {
      if (this.activeIndex < 0) return null;
      return this.filteredContacts[this.activeIndex];
    },
    filteredContacts() {
      if (!this.searchText) return this.contacts;
      return this.contacts.filter((_contact, index) =>
        this.contactStrings[index].includes(this.searchText)
      );
    },
    activeContact() {
      if (this.activeIndex < 0) return null;
      return this.filteredContacts[this.activeIndex];
    },
    filteredContactsCount() {
      return this.filteredContacts.length;
    },
  },
  methods: {
    async getNovel(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (error) {
        console.log(error);
        this.$router.push({
          name: "notFound",
          params: {
            pathMatch: this.$route.path.split("/").slice(0),
          },
          query: this.$route.query,
          hash: this.$route.hash,
        });
      }
    },
    async retrieveContacts() {
      try {
        this.contacts = await ContactService.getAll();
      } catch (error) {
        console.log(error);
      }
    },
    refreshList() {
      this.retrieveContacts();
      this.activeIndex = -1;
    },
  },
  created() {
    this.getNovel(this.id);
    this.message = "";
  },
  mounted() {
    this.refreshList();
  },
};
</script>
<style scoped>
.page {
  text-align: left;
  max-width: 750px;
}
</style>
