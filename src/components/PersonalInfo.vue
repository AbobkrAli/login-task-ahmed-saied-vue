<!-- src/components/PersonalInfo.vue -->
<template>
  <div class="p-4 space-y-4">
    <div class="space-y-2">
      <label for="name" class="block text-gray-700 font-medium">الاسم</label>
      <input
        id="name"
        :value="name"
        @input="$emit('update:name', $event.target.value)"
        type="text"
        placeholder="أدخل اسمك"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.name }"
      />
      <div v-if="errors.name" class="text-red-500 text-sm">{{ errors.name }}</div>

      <label for="email" class="block text-gray-700 font-medium">الايميل</label>
      <input
        id="email"
        :value="email"
        @input="$emit('update:email', $event.target.value)"
        type="email"
        placeholder="أدخل ايميلك"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.email }"
      />
      <div v-if="errors.email" class="text-red-500 text-sm">{{ errors.email }}</div>

      <label for="phone" class="block text-gray-700 font-medium">الهاتف</label>
      <input
        id="phone"
        :value="phone"
        @input="$emit('update:phone', $event.target.value)"
        type="number"
        placeholder="أدخل رقم هاتفك"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.phone }"
      />
      <div v-if="errors.phone" class="text-red-500 text-sm">{{ errors.phone }}</div>
    </div>

    <button
      @click="nextStep"
      class="bg-red-500 text-white p-3 rounded-lg w-full hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-500"
    >
      التال
    </button>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    email: String,
    phone: String,
    errors: Object
  },
  methods: {
    nextStep() {
      this.$emit('nextStep');
    }
  }
};
</script>

<style scoped>
/* Scoped styles (if needed) */
</style>
