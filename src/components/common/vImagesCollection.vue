<script setup lang="ts">
import { computed, ref } from 'vue'
import { IResponseGetAllAvatars } from '../../api/avatarController/avatar.api.interfaces'
import { Nullable } from '../../types/testsTypes.interface'

const props = defineProps<{
  srcArray: Nullable<IResponseGetAllAvatars[]>;
}>()
const emit = defineEmits<{(e: 'clickImage', target: string): void }>()

const cardDimension = ref(400)
const cardDimensionToPixel = computed(() => cardDimension.value + 'px')

const isActiveCardId = ref<null | string>(null)
const clickOnImage = (id: string) => {
  if (isActiveCardId.value !== id) {
    emit('clickImage', id)
    isActiveCardId.value = id
  }
}
</script>
<template>
  <div
    v-if="srcArray"
    class="image-collection"
  >
    <div
      v-for="item in props.srcArray"
      :key="item._id"
      class="image-collection_item"
      @click.prevent="clickOnImage(item._id)"
    >
      <img
        class="image-collection_item__image"
        :src="item.base_link"
        alt="image collection item"
        :width="cardDimension / 2"
      >
      <h2 class="image-collection_item__title">
        {{ item.ref_name.toUpperCase() }}
      </h2>
    </div>
  </div>
  <div
    v-else
    class="image-collection_item image-collection_skeleton"
  />
</template>
<style lang="postcss">
.image-collection {
  --title-height: 6rem;
  display: flex;
  gap: 1rem;
  align-items: flex-end;
  &_item {
    border-radius: var(--border-prime);
    backdrop-filter: blur(3rem);
    cursor: pointer;

    background: hsla(0, 3%, 23%, 0.4);

    display: flex;
    flex-direction: column;
    border: 1px solid var(--color-violet);
    &__title {
      margin: 0;
      height: var(--title-height);

      display: flex;
      align-items: center;
      justify-content: center;

      font-family: var(--neon-font);
    }
    &:hover {
      background: var(--gradient);
    }
  }
  &_skeleton {
    --dimension: v-bind("cardDimensionToPixel");
    height: calc(var(--dimension) + var(--title-height));
    width: var(--dimension);
  }
}
</style>
