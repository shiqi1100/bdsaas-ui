<template>
  <div class="button-box">
    <dz-button
      :size="size"
      :type="type"
      :round="round"
      :disabled="disabled"
      icon="icon-icon-test1"
      @click="btnHandler"
    >
      Hello World
      <template #icon>
        <i class="iconfont icon-icon-test"></i>
      </template>
    </dz-button>
    <dz-button-setup
      :size="size"
      :type="type"
      :round="round"
      :disabled="disabled"
      icon="icon-icon-test1"
    >
      Hello World（setup）
      <template #icon>
        <i class="iconfont icon-icon-test1"></i>
      </template>
    </dz-button-setup>
  </div>

  <div class="box">
    <dz-button type="default" @click="setConfig('size')">设置尺寸</dz-button>
    <dz-button type="default" @click="setConfig('type')">设置类型</dz-button>
    <dz-button type="default" @click="setConfig('round')">设置圆角</dz-button>
    <dz-button type="default" @click="setConfig('disabled')"
      >设置禁用
    </dz-button>
  </div>
  <hr />
  <h3>单元测试组件</h3>
  <hello-test :count="4" />
</template>

<script setup lang="ts">
import { reactive, toRefs, unref } from 'vue'
import HelloTest from '../../test/HelloTest.vue'
import type { Ref } from 'vue'
import type { ButtonSize, ButtonType } from '@/components/button/src/interface'
import DzButton from '@/components/button/src/button.vue'
import DzButtonSetup from '@/components/button/src/button-setup.vue'

type ArgType = 'size' | 'type' | 'round' | 'disabled'

interface BtnConfig {
  size: ButtonSize
  type: ButtonType
  round: boolean
  disabled: boolean
}

const btnConfig = reactive<BtnConfig>({
  size: 'default',
  type: 'primary',
  round: true,
  disabled: false
})
const configAsRefs = toRefs(btnConfig)

const { size, type, round, disabled } = configAsRefs

// fixme 3.使用泛型约束只能入我们想要的参数
function setValue<T, U extends T>(data: Ref<T>, value: U) {
  data.value = value
}

const setHandler = {
  size: () =>
    setValue<ButtonSize, ButtonSize>(
      size,
      unref(size) === 'default' ? 'small' : 'default'
    ),
  type: () =>
    setValue<ButtonType, ButtonType>(
      type,
      unref(type) === 'primary' ? 'default' : 'primary'
    ),
  round: () => setValue<boolean, boolean>(round, !unref(round)),
  disabled: () => setValue<boolean, boolean>(disabled, !unref(disabled))
}

function setConfig(type: ArgType) {
  setHandler[type]()
}

function btnHandler() {
  console.log(999)
}
</script>

<style>
.box {
  display: flex;
  justify-content: space-between;
  width: 500px;
  margin: 50px auto;
}

.button-box {
  display: flex;
  justify-content: space-between;
  width: 420px;
  margin: 0 auto;
}
</style>
