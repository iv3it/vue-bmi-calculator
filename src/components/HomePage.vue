<template>
  <div class="container">
    <div class="d-flex justify-content-center pt-5 pb-3">
      <h1>BMI calculator</h1>
    </div>

    <section class="bmi">
      <h2 class="bmi__text textOverflow"><span class="textOverflow__span" id="bmiValue">{{ bmiValue }}</span> <span class="bmi__text bmi__text--xs">BMI</span></h2>
      <h3 class="bmi__text bmi__text--sm textOverflow"><span class="textOverflow__span" id="bmiValueDescription">{{ bmiValueDescription }}</span></h3>
    </section>

    <form class="form container my-4 p-0">
      <div class="row">
        <div class="col-sm-6 col-12">
          <div class="form__box">
            <label for="weight" class="form__label">Weight</label>
            <p class="form__text form__text--blue mb-3">{{ form.weight }}<span class="form__text--blue form__text--xs ms-1">kg</span></p>
            <input class="form__range" id="weight" type="range" min="30" max="200" step="0.5" v-model="form.weight" required>
            <div class="w-100 d-flex justify-content-between">
              <p class="form__text--black form__text--xs">30</p>
              <p class="form__text--black form__text--xs">200</p>
            </div>
          </div>
        </div>
        <div class="col-sm-6 col-12 mt-sm-0 mt-3">
          <div class="form__box">
            <label for="height" class="form__label">Height</label>
            <p class="form__text form__text--blue mb-3">{{ form.height }}<span class="form__text--blue form__text--xs ms-1">cm</span></p>
            <input class="form__range" id="height" type="range" min="70" max="230" step="1" v-model="form.height" required>
            <div class="w-100 d-flex justify-content-between">
              <p class="form__text--black form__text--xs">70</p>
              <p class="form__text--black form__text--xs">230</p>
            </div>
          </div>
        </div>
      </div>
    </form>
    
  </div>
</template>

<script>
import { reactive, computed } from '@vue/reactivity'
import { watch } from '@vue/runtime-core';
import gsap from "gsap";

export default {
  name: 'HomePage',
  setup() {
    let form = reactive({
      weight: 50,
      height: 150,
    })

    let bmiValue = computed(() =>
      Math.round(form.weight / ((form.height / 100) ** 2) * 10) / 10
    );

    let bmiValueDescription = computed(() => {
      let x = Math.round(form.weight / ((form.height / 100) ** 2) * 10) / 10;

      if (x < 18.5) {
        return "Underweight"
      } else if (x >= 18.5 && x <= 24.9) {
        return "Normal weight"
      } else if (x >= 25 && x <= 29.9) {
        return "Overweight"
      } else if (x >= 30) {
        return "Obese"
      }
    });

    watch(form, () => {
      gsap.fromTo("#bmiValue", { 
        transform: "translateY(100%)"
      }, {
        duration: 0.6,
        transform: "translateY(0)"
      });

      gsap.fromTo("#bmiValueDescription", { 
        transform: "translateY(100%)"
      }, {
        duration: 0.6,
        delay: 0.5,
        transform: "translateY(0)"
      });
    });

    return {
      form, bmiValue, bmiValueDescription
    }
  }
}
</script>

<style scoped lang="scss">
@import '../variables';

.bmi {
  position: relative;
  margin: 2rem 0;
  padding: 2rem;
  background-color: $blue;
  border-radius: 10px;
  overflow: hidden;

  &__text {
    position: relative;
    font-size: 5rem;
    font-weight: 700;
    text-align: center;
    color: $white;
    margin-bottom: 0;

    @media (max-width: 575px) {
      font-size: 3rem;
    }

    &--sm {
      font-size: 1.3rem;
      font-weight: 500;
      text-transform: uppercase;

      @media (max-width: 575px) {
        font-size: 1.2rem;
      }
    }

    &--xs {
      font-size: 1.2rem;

      @media (max-width: 575px) {
        font-size: 0.8rem;
      }
    }

    &--blue {
      color: $blue;
    }
  }
}

.form {
  &__box {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border-radius: 10px;
    background-color: #fff;
    padding: 1.5rem;
    box-shadow: 0px 0px 5px 0px rgba(210, 210, 210, 1);
  }

  &__text {
    font-size: 3.5rem;
    font-weight: 700;
    text-align: center;
    color: $white;
    margin-bottom: 0;

    &--sm {
      font-size: 1.5rem;
      font-weight: 500;
      text-transform: uppercase;
    }

    &--xs {
      font-size: 0.9rem;
    }

    &--blue {
      color: $blue;
    }

    &--black {
      color: $black;
    }
  }

  &__label {
    color: $black;
    font-size: 1.2rem;
    font-weight: 500;
  }

  &__input {
    border: 1px solid $black;
    border-radius: 5px;
    padding: 0.2rem 1rem;
  }

  &__range {
    -webkit-appearance: none;
    appearance: none;
    width: 100%;
    height: 10px;
    background: #d3d3d3;
    border-radius: 8px;
    outline: none;
    opacity: 0.7;
    -webkit-transition: .2s;
    transition: opacity .2s;
    

    &:hover {
      opacity: 1;
    }

    &::-moz-range-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 20px;
      height: 20px;
      border-radius: 50%; 
      background: $blue;
      border: none;
      cursor: pointer;      
    }

    &::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 20px;
      height: 20px;
      border-radius: 50%; 
      background: $blue;
      border: none;
      cursor: pointer;      
    }
  }
}

.textOverflow {
  position: relative;
  overflow: hidden;

  &__span {
    display: inline-block;
  }
}

</style>
