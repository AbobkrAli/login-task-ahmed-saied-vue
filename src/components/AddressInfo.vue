<!-- src/components/AddressInfo.vue -->
<template>
  <div class="p-4 space-y-4">
    <div class="space-y-2">
      <label for="street" class="block text-gray-700 font-medium">اسم الشارع</label>
      <input
        id="street"
        :value="street"
        @input="$emit('update:street', $event.target.value)"
        type="text"
        placeholder="أدخل اسم الشارع"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.street }"
      />
      <div v-if="errors.street" class="text-red-500 text-sm">{{ errors.street }}</div>

      <label for="city" class="block text-gray-700 font-medium">المدينة</label>
      <input
        id="city"
        :value="city"
        @input="$emit('update:city', $event.target.value)"
        type="text"
        placeholder="أدخل المدينة"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.city }"
      />
      <div v-if="errors.city" class="text-red-500 text-sm">{{ errors.city }}</div>

      <label for="state" class="block text-gray-700 font-medium">الولاية</label>
      <input
        id="state"
        :value="state"
        @input="$emit('update:state', $event.target.value)"
        type="text"
        placeholder="أدخل الولاية"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.state }"
      />
      <div v-if="errors.state" class="text-red-500 text-sm">{{ errors.state }}</div>

      <label for="zip" class="block text-gray-700 font-medium">الرمز البريدي</label>
      <input
        id="zip"
        :value="zip"
        @input="$emit('update:zip', $event.target.value)"
        type="text"
        placeholder="أدخل الرمز البريدي"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.zip }"
      />
      <div v-if="errors.zip" class="text-red-500 text-sm">{{ errors.zip }}</div>
    </div>

    <div class="flex space-x-2 gap-8">
      <button
        @click="prevStep"
        class="bg-gray-500 text-white p-3 rounded-lg w-full hover:bg-gray-300 focus:outline-none focus:ring-2 focus:ring-gray-300"
      >
        رجوع
      </button>
      <button
        @click="nextStep"
        class="bg-red-500 text-white p-3 rounded-lg w-full hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-500"
      >
        التالي
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    street: String,
    city: String,
    state: String,
    zip: String,
    errors: Object
  },
  methods: {
    nextStep() {
      this.$emit('nextStep');
    },
    prevStep() {
      this.$emit('prevStep');
    }
  }
};
</script>

<style scoped>
/* Scoped styles (if needed) */
</style>
