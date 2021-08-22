<template>
  <form class="validate-form-container">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="submitForm">
      <slot name="submit">
        <button type="submit" class="btn btn-primary">提交</button>
      </slot>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, onUnmounted } from 'vue';
import mitt from 'mitt';
type Events = { 'form-item-created': string; value: string };
export const emitter = mitt<Events>();
export default defineComponent({
  emits: ['form-submit'],
  setup(props, context) {
    const submitForm = () => {
      context.emit('form-submit', true);
    };
    const callback = (test?: string) => {
      console.log(test);
    };
    // 监听自组件响应事件
    emitter.on('form-item-created', callback);
    onUnmounted(() => {
      // 组件销毁时，解除监听子组件的响应事件
      emitter.off('form-item-created', callback);
    });
    return {
      submitForm
    };
  }
});
</script>