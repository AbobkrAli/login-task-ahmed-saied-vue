<template>
  <div class="container mx-auto max-w-[500px] flex flex-col items-center bg-white rounded-lg p-6 relative min-h-[600px]"  dir="rtl">
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
          :name="name"
          :email="email"
          :phone="phone"
          :errors="errors"
          @update:name="name = $event"
          @update:email="email = $event"
          @update:phone="phone = $event"
          @nextStep="handleNextStep"
        />
        <AddressInfo
          v-if="step === 2"
          :street="street"
          :city="city"
          :state="state"
          :zip="zip"
          :errors="errors"
          @update:street="street = $event"
          @update:city="city = $event"
          @update:state="state = $event"
          @update:zip="zip = $event"
          @nextStep="handleNextStep"
          @prevStep="handlePrevStep"
        />
        <PaymentInfo
          v-if="step === 3"
          :cardNumber="cardNumber"
          :expiryDate="expiryDate"
          :cvv="cvv"
          :errors="errors"
          @update:cardNumber="cardNumber = $event"
          @update:expiryDate="expiryDate = $event"
          @update:cvv="cvv = $event"
          @submit="handleSubmit"
          @prevStep="handlePrevStep"
        />
      </div>
    </transition>
  </div>
</template>
<script>
import PersonalInfo from '../components/PersonalInfo.vue';
import AddressInfo from '../components/AddressInfo.vue';
import PaymentInfo from '../components/PaymentInfo.vue';

export default {
  components: {
    PersonalInfo,
    AddressInfo,
    PaymentInfo
  },
  data() {
    return {
      step: 1,
      name: '',
      email: '',
      phone: '',
      street: '',
      city: '',
      state: '',
      zip: '',
      cardNumber: '',
      expiryDate: '',
      cvv: '',
      errors: {},
      slideDirection: 'slide-left',
      indicatorWidth: '0' // Initial width for the first step
    };
  },
  methods: {
    handleNextStep() {
    this.validateCurrentStep();
    if (Object.keys(this.errors).length === 0) {
      this.slideDirection = 'slide-right'; // Change to 'slide-right' for next step
      this.step++;
    }
  },
  handlePrevStep() {
    this.slideDirection = 'slide-left'; // Change to 'slide-left' for previous step
    this.step--;
  },
  goToStep(targetStep) {
    if (targetStep >= 1 && targetStep <= 3) {
      this.slideDirection = targetStep > this.step ? 'slide-right' : 'slide-left'; // Adjust direction
      this.step = targetStep;
    }
  },
  beforeEnter(el) {
    el.style.transform = this.slideDirection === 'slide-left' ? 'translateX(100%)' : 'translateX(-100%)';
  },
  enter(el, done) {
    el.offsetHeight; // trigger reflow
    el.style.transition = 'transform 0.5s ease';
    el.style.transform = 'translateX(0)';
    done();
  },
  leave(el, done) {
    el.style.transition = 'transform 0.5s ease';
    el.style.transform = this.slideDirection === 'slide-left' ? 'translateX(100%)' : 'translateX(-100%)';
    done();
  },
    handleSubmit() {
      this.validateCurrentStep();
      if (Object.keys(this.errors).length === 0) {
        // All validations passed, proceed with form submission
        console.log('Form Submitted', {
          name: this.name,
          email: this.email,
          phone: this.phone,
          street: this.street,
          city: this.city,
          state: this.state,
          zip: this.zip,
          cardNumber: this.cardNumber,
          expiryDate: this.expiryDate,
          cvv: this.cvv
        });
      }
    },
    goToStep(targetStep) {
      if (targetStep >= 1 && targetStep <= 3) {
        this.slideDirection = targetStep > this.step ? 'slide-left' : 'slide-right';
        this.step = targetStep;
      }
    },
    validateCurrentStep() {
      this.errors = {};
      
      if (this.step === 1) {
        if (!this.name) this.errors.name = 'الاسم مطلوب';
        if (!this.email) this.errors.email = 'الايميل مطلوب';
        if (!this.phone) this.errors.phone = 'الهاتف مطلوب';
      }
      
      if (this.step === 2) {
        if (!this.street) this.errors.street = 'اسم الشارع مطلوب';
        if (!this.city) this.errors.city = 'المدينة مطلوبة';
        if (!this.state) this.errors.state = 'الولاية مطلوبة';
        if (!this.zip) this.errors.zip = 'الرمز البريدي مطلوب';
      }
      
      if (this.step === 3) {
        if (!this.cardNumber) this.errors.cardNumber = 'رقم الفيزا مطلوب';
        if (!this.expiryDate) this.errors.expiryDate = 'تاريخ انتهاء الصلاحية مطلوب';
        if (!this.cvv) this.errors.cvv = 'رمز التحقق مطلوب';
      }
    },
    
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
</style>
