<template>
  <div class="container mx-auto max-w-[500px] flex flex-col items-center bg-white rounded-lg p-6 relative min-h-[600px]" dir="rtl">
    <!-- Step Indicators -->
    <div class="steps mb-6 flex justify-between w-full relative">
      <div
        v-for="i in 3"  
        :key="i"
        :class="['step', { 'active': step >= i }]"
      >
        {{ i }}
      </div>
    </div>

    <hr class="h-[1px] my-[10px] w-full bg-gray-300">

    <!-- Form Content -->
    <transition :name="slideDirection" @before-enter="beforeEnter" @enter="enter" @leave="leave">
      <div :key="step" class="form-step">
        <PersonalInfo
          v-if="step === 1"
          :name="name.value"
          :email="email.value"
          :phone="phone.value"
          :errors="errors"
          @update:name="updateName"
          @update:email="updateEmail"
          @update:phone="updatePhone"
          @nextStep="handleNextStep"
        />
        <AddressInfo
          v-if="step === 2"
          :street="street.value"
          :city="city.value"
          :state="state.value"
          :zip="zip.value"
          :errors="errors"
          @update:street="updateStreet"
          @update:city="updateCity"
          @update:state="updateState"
          @update:zip="updateZip"
          @nextStep="handleNextStep"
          @prevStep="handlePrevStep"
        />
        <PaymentInfo
          v-if="step === 3"
          :cardNumber="cardNumber.value"
          :expiryDate="expiryDate.value"
          :cvv="cvv.value"
          :errors="errors"
          @update:cardNumber="updateCardNumber"
          @update:expiryDate="updateExpiryDate"
          @update:cvv="updateCvv"
          @submit="handleSubmit"
          @prevStep="handlePrevStep"
        />
      </div>
    </transition>

    <!-- Step Navigation Buttons -->
    
  </div>
</template>

<script>
import { ref } from 'vue';
import PersonalInfo from '../components/PersonalInfo.vue';
import AddressInfo from '../components/AddressInfo.vue';
import PaymentInfo from '../components/PaymentInfo.vue';

export default {
  components: {
    PersonalInfo,
    AddressInfo,
    PaymentInfo
  },
  setup() {
    
    const step = ref(1);
    const name = ref('');
    const email = ref('');
    const phone = ref('');
    const street = ref('');
    const city = ref('');
    const state = ref('');
    const zip = ref('');
    const cardNumber = ref('');
    const expiryDate = ref('');
    const cvv = ref('');
    const errors = ref({});
    const slideDirection = ref('slide-left');

    
    const updateName = (value) => { name.value = value; };
    const updateEmail = (value) => { email.value = value; };
    const updatePhone = (value) => { phone.value = value; };
    const updateStreet = (value) => { street.value = value; };
    const updateCity = (value) => { city.value = value; };
    const updateState = (value) => { state.value = value; };
    const updateZip = (value) => { zip.value = value; };
    const updateCardNumber = (value) => { cardNumber.value = value; };
    const updateExpiryDate = (value) => { expiryDate.value = value; };
    const updateCvv = (value) => { cvv.value = value; };

    // Navigation methods
    const handleNextStep = () => {
      validateCurrentStep();
      if (Object.keys(errors.value).length === 0) {
        slideDirection.value = 'slide-right';
        step.value++;
      }
    };

    const handlePrevStep = () => {
      slideDirection.value = 'slide-left';
      step.value--;
    };

    const goToStep = (targetStep) => {
      if (targetStep >= 1 && targetStep <= 3) {
        slideDirection.value = targetStep > step.value ? 'slide-right' : 'slide-left';
        step.value = targetStep;
      }
    };

    // Form submission handling
    const handleSubmit = () => {
      validateCurrentStep();
      if (Object.keys(errors.value).length === 0) {
        console.log('Form Submitted', {
          name: name.value,
          email: email.value,
          phone: phone.value,
          street: street.value,
          city: city.value,
          state: state.value,
          zip: zip.value,
          cardNumber: cardNumber.value,
          expiryDate: expiryDate.value,
          cvv: cvv.value
        });
      }
    };

    // Validation method for the current step
    const validateCurrentStep = () => {
      errors.value = {};

      if (step.value === 1) {
        if (!name.value) errors.value.name = 'الاسم مطلوب';
        if (!email.value) errors.value.email = 'الايميل مطلوب';
        if (!phone.value) errors.value.phone = 'الهاتف مطلوب';
      }
      
      if (step.value === 2) {
        if (!street.value) errors.value.street = 'اسم الشارع مطلوب';
        if (!city.value) errors.value.city = 'المدينة مطلوبة';
        if (!state.value) errors.value.state = 'الولاية مطلوبة';
        if (!zip.value) errors.value.zip = 'الرمز البريدي مطلوب';
      }
      
      if (step.value === 3) {
        if (!cardNumber.value) errors.value.cardNumber = 'رقم الفيزا مطلوب';
        if (!expiryDate.value) errors.value.expiryDate = 'تاريخ انتهاء الصلاحية مطلوب';
        if (!cvv.value) errors.value.cvv = 'رمز التحقق مطلوب';
      }
    };

    // Transition methods
    const beforeEnter = (el) => {
      el.style.transform = slideDirection.value === 'slide-left' ? 'translateX(100%)' : 'translateX(-100%)';
    };

    const enter = (el, done) => {
      el.offsetHeight; // trigger reflow
      el.style.transition = 'transform 0.5s ease';
      el.style.transform = 'translateX(0)';
      done();
    };

    const leave = (el, done) => {
      el.style.transition = 'transform 0.5s ease';
      el.style.transform = slideDirection.value === 'slide-left' ? 'translateX(100%)' : 'translateX(-100%)';
      done();
    };

    
    return {
      step,
      name,
      email,
      phone,
      street,
      city,
      state,
      zip,
      cardNumber,
      expiryDate,
      cvv,
      errors,
      slideDirection,
      handleNextStep,
      handlePrevStep,
      goToStep,
      handleSubmit,
      beforeEnter,
      enter,
      leave,
      updateName,
      updateEmail,
      updatePhone,
      updateStreet,
      updateCity,
      updateState,
      updateZip,
      updateCardNumber,
      updateExpiryDate,
      updateCvv
    };
  }
};
</script>

<style scoped>
.container {
  position: relative;
  overflow: hidden;
  padding-top: 50px; /* Ensure enough space for step indicators */
}

.step {
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  border: 2px solid #ddd;
  background-color: #f5f5f5;
  cursor: pointer;
  position: relative;
  z-index: 1; /* Ensure steps are above the indicator */
}

.step.active {
  background-color: #e7194d;
  color: #fff;
  border: 2px solid #e7194d;
}

.form-step {
  position: relative; /* Ensure proper stacking */
  width: 100%;
  height: 100%; /* Ensure it takes the available space */
}

.slide-left-enter-active,
.slide-left-leave-active {
  transition: transform 0.2s ease;
}

.slide-left-enter,
.slide-left-leave-to {
  transform: translateX(-100%);
}

.slide-right-enter-active,
.slide-right-leave-active {
  transition: transform 0.2s ease;
}

.slide-right-enter,
.slide-right-leave-to {
  transform: translateX(100%);
}

/* Styles for step navigation buttons */
.flex {
  margin-top: 20px;
}
</style>
