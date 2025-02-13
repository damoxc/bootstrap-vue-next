<template>
  <BCard :img-bottom="imgBottom">
    <template v-if="!noImgBoolean" #img>
      <slot name="img">
        <BCardImg
          :blank="!imgSrc ? true : false"
          :blank-color="imgBlankColor"
          :height="!imgSrc ? imgHeight : undefined"
          :src="imgSrc"
          :top="!imgBottom"
          :bottom="imgBottom"
        />
      </slot>
    </template>

    <template v-if="!noHeaderBoolean" #header>
      <slot name="header">
        <BPlaceholder
          :width="headerWidth"
          :variant="headerVariant"
          :animation="headerAnimation"
          :size="headerSize"
        />
      </slot>
    </template>
    <slot>
      <BPlaceholder cols="7" v-bind="defaultAttrs" />
      <BPlaceholder cols="4" v-bind="defaultAttrs" />
      <BPlaceholder cols="4" v-bind="defaultAttrs" />
      <BPlaceholder cols="6" v-bind="defaultAttrs" />
      <BPlaceholder cols="8" v-bind="defaultAttrs" />
    </slot>
    <template v-if="!noFooterBoolean" #footer>
      <slot name="footer">
        <component
          :is="footerComponent"
          :width="footerWidth"
          :animation="footerAnimation"
          :size="noButtonBoolean ? footerSize : undefined"
          :variant="footerVariant"
        />
      </slot>
    </template>
  </BCard>
</template>

<script setup lang="ts">
import BCard from '../BCard/BCard.vue'
import BCardImg from '../BCard/BCardImg.vue'
import BPlaceholder from './BPlaceholder.vue'
import BPlaceholderButton from './BPlaceholderButton.vue'
import type {Booleanish, ColorVariant, PlaceholderAnimation, PlaceholderSize} from '../../types'
import {computed, toRef} from 'vue'
import {useBooleanish} from '../../composables'

const props = withDefaults(
  defineProps<{
    noHeader?: Booleanish
    headerWidth?: string | number
    headerVariant?: ColorVariant | null
    headerAnimation?: PlaceholderAnimation
    headerSize?: PlaceholderSize
    noFooter?: Booleanish
    footerWidth?: string | number
    footerVariant?: ColorVariant | null
    footerAnimation?: PlaceholderAnimation
    footerSize?: PlaceholderSize
    animation?: PlaceholderAnimation
    size?: PlaceholderSize
    variant?: ColorVariant | null
    noButton?: Booleanish
    imgBottom?: Booleanish
    imgSrc?: string
    imgBlankColor?: string
    imgHeight?: string | number
    noImg?: Booleanish
  }>(),
  {
    footerVariant: undefined,
    footerAnimation: undefined,
    footerSize: 'md',
    animation: undefined,
    size: 'md',
    variant: undefined,
    imgSrc: undefined,
    headerAnimation: undefined,
    headerSize: 'md',
    headerVariant: undefined,
    noButton: false,
    headerWidth: 100,
    footerWidth: 100,
    noHeader: false,
    noFooter: false,
    imgBlankColor: '#868e96',
    imgHeight: 100,
    imgBottom: false,
    noImg: false,
  }
)

defineSlots<{
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  default?: (props: Record<string, never>) => any
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  img?: (props: Record<string, never>) => any
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  header?: (props: Record<string, never>) => any
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  footer?: (props: Record<string, never>) => any
}>()

const noButtonBoolean = useBooleanish(() => props.noButton)
const noHeaderBoolean = useBooleanish(() => props.noHeader)
const noFooterBoolean = useBooleanish(() => props.noFooter)
const noImgBoolean = useBooleanish(() => props.noImg)

const defaultAttrs = computed(() => ({
  animation: props.animation,
  size: props.size,
  variant: props.variant,
}))

const footerComponent = toRef(() => (!noButtonBoolean.value ? BPlaceholderButton : BPlaceholder))
</script>
