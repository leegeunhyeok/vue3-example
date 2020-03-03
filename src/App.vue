<template>
  <div id="app" class="app">
    <h1>Count: {{ countWithUnit }}</h1>
    <div class="app__counter">
      <button class="red" @click="decrease">-</button>
      <button class="blue" @click="increase">+</button>
    </div>
    <div class="app__form">
      <input type="text" v-model="name" placeholder="Name">
      <input type="number" v-model="detail.age" placeholder="Age">
    </div>
    <div class="app__control">
      <button @click="toggleState">Toggle Modal</button>
    </div>
    <!-- 모달 컴포넌트 -->
    <Modal :name="name" :age="detail.age" v-if="showModal"/>
  </div>
</template>

<script>
import {
  ref, reactive, toRefs, watch, computed, onMounted
} from '@vue/composition-api'

import Modal from '@/components/Modal'

const useCount = () => {
  // 해당 값을 참조하는 변경 가능한 반응형 객체를 반환
  const count = ref(0)
  // getter가 반환하는 값에 대한 변경 불가능(Immutable)한 반응형 객체를 반환
  const countWithUnit = computed(() => count.value + ' 입니다!')

  // ref 객체는 value 프로퍼티를 통해 값에 접근할 수 있음
  const increase = () => ++count.value
  const decrease = () => --count.value

  return { countWithUnit, increase, decrease }
}

const useModalState = () => {
  const showModal = ref(false)

  const toggleState = () => {
    showModal.value = !showModal.value
  }

  return {
    showModal,
    toggleState
  }
}

const useFormData = () => {
  // 원본 참조 데이터
  const refData = {
    name: '',
    detail: {
      age: 0
    }
  }

  /**
   * reactive는 원본 객체와 완전히 동일한 반응형 프록시를 반환하지만
   * ES6의 프록시는 원본 객체와 다르다(참조)
   */
  const formData = reactive(refData)
  const proxyData = new Proxy(refData, {})

  console.log(refData === formData) // true
  console.log(refData === proxyData) // false

  // getter를 통해 변경 감지할 값을 받으며, 해당 값이 변경되는 경우 콜백 호출
  watch(() => formData.detail.age, (newVal, prevVal) => {
    console.log(`New: ${newVal} (old: ${prevVal})`)
  })

  return toRefs(formData)
}

export default {
  name: 'app',
  components: {
    Modal
  },
  setup () {
    const { countWithUnit, increase, decrease } = useCount()
    const { showModal, toggleState } = useModalState()
    const { name, detail } = useFormData()

    // 생명주기 훅 사용 가능
    onMounted(() => {
      console.log('Mounted!!')
    })

		return {
			countWithUnit,
      increase,
      decrease,
      showModal,
      toggleState,
      name,
      detail
    }
  }
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
}

.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  &__counter,
  &__control {
    width: 100%;
    padding: .5rem;
  }

  button {
    cursor: pointer;
    outline: none;
    width: 100%;
    padding: .4rem .8rem;
    border: 0;
    font-size: 1rem;
    box-shadow: 0 0 5px rgba(0, 0, 0, .25);
    transition: .2s;

    &:hover {
      box-shadow: 0 0 10px rgba(0, 0, 0, .25);
    }

    &.red {
      width: 50%;
      background-color: tomato;
      color: #fff;
    }

    &.blue {
      width: 50%;
      background-color: dodgerblue;
      color: #fff;
    }
  }

  input {
    outline: none;
    padding: .4rem .8rem;
    margin: .5rem;
    border: 0;
    font-size: 1rem;
    box-shadow: 0 0 5px rgba(0, 0, 0, .25);
  }
}
</style>
