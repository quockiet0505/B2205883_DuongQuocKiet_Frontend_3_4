<template>
     <div v-if="contact" class="page">
         <h4>Hiệu chỉnh Liên hệ</h4>
         <ContactForm
              :contact="contact"
              :showDelete="!!id"
              @submit:contact="updateContact"
              @delete:contact="deleteContact"
         />
         <p>{{ message }}</p>
     </div>
     <div v-else class="page">
         <h4>Thêm mới liên hệ</h4>
         <ContactForm @submit:contact="createContact" />
         <p>{{ message }}</p>
     </div>
</template>
<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";
export default {
     components: { ContactForm },
     props: { id: { type: String, required: true } },
     data() {
         return { contact: null, message: "" };
     },
     methods: {
         async getContact(id) {
             try {
                 if (id) {
                     const res = await ContactService.get(id);
                     const data = res?.data ?? res;
                     if (data && !data._id && data.id) data._id = data.id;
                     this.contact = data;
                 }
             } catch (error) {
                 console.log(error);
                 this.$router.push({
                     name: "notfound",
                     params: { pathMatch: this.$route.path.split("/").slice(1) },
                     query: this.$route.query,
                     hash: this.$route.hash,
                 });
             }
         },
         async createContact(data) {
             try {
                 await ContactService.create(data);
                 alert('Liên hệ thêm thành công');
                 this.$router.push({ name: "contactbook" });
             } catch (error) {
                 console.log(error);
             }
         },
         async updateContact(data) {
             try {
                 await ContactService.update(this.id, data);
                 alert('Liên hệ được cập nhật thành công.');
                 this.$router.push({ name: "contactbook" });
             } catch (error) {
                 console.log(error);
             }
         },
         async deleteContact() {
             if (confirm("Bạn muốn xóa Liên hệ này?")) {
                 try {
                     await ContactService.delete(this.id);
                     this.$router.push({ name: "contactbook" });
                 } catch (error) {
                     console.log(error);
                 }
             }
         },
     },
     created() {
         this.getContact(this.id);
         this.message = "";
     },
};
</script>