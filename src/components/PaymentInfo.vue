<!-- src/components/PaymentInfo.vue -->
<template>
  <div class="p-4 space-y-4">
    <div class="space-y-2">
      <label for="cardNumber" class="block text-gray-700 font-medium">رقم الفيزا</label>
      <input
        id="cardNumber"
        :value="cardNumber"
        @input="$emit('update:cardNumber', $event.target.value)"
        type="text"
        placeholder="أدخل رقم الفيزا"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.cardNumber }"
      />
      <div v-if="errors.cardNumber" class="text-red-500 text-sm">{{ errors.cardNumber }}</div>

      <label for="expiryDate" class="block text-gray-700 font-medium">تاريخ انتهاء الصلاحية</label>
      <input
        id="expiryDate"
        :value="expiryDate"
        @input="$emit('update:expiryDate', $event.target.value)"
        type="text"
        placeholder="أدخل تاريخ انتهاء الصلاحية"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.expiryDate }"
      />
      <div v-if="errors.expiryDate" class="text-red-500 text-sm">{{ errors.expiryDate }}</div>

      <label for="cvv" class="block text-gray-700 font-medium">رمز التحقق</label>
      <input
        id="cvv"
        :value="cvv"
        @input="$emit('update:cvv', $event.target.value)"
        type="text"
        placeholder="أدخل رمز التحقق"
        class="border rounded-lg p-3 w-full mb-2 focus:outline-none focus:ring-2 focus:ring-red-500"
        :class="{ 'border-red-500': errors.cvv }"
      />
      <div v-if="errors.cvv" class="text-red-500 text-sm">{{ errors.cvv }}</div>
    </div>

    <div class="flex space-x-2 gap-8">
      <button
        @click="prevStep"
        class="bg-gray-400 text-white p-3 rounded-lg w-full hover:bg-gray-500 focus:outline-none focus:ring-2 focus:ring-gray-300"
      >
        رجوع
      </button>
      <button
        @click="submit"
        class="bg-red-500 text-white p-3 rounded-lg w-full hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-500"
      >
        ارسال
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cardNumber: String,
    expiryDate: String,
    cvv: String,
    errors: Object
  },
  methods: {
    submit() {
      this.$emit('submit');
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
