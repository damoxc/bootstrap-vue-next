<template>
  <component :is="tag" class="card" :class="computedClasses">
    <slot v-if="!imgBottomBoolean" name="img">
      <BCardImg v-if="imgSrc" v-bind="imgAttr" />
    </slot>
    <BCardHeader
      v-if="header || hasHeaderSlot || headerHtml"
      :bg-variant="headerBgVariant"
      :variant="headerVariant"
      :border-variant="headerBorderVariant"
      :html="headerHtml"
      :tag="headerTag"
      :text-variant="headerTextVariant"
      :class="headerClass"
    >
      <slot name="header">
        {{ header }}
      </slot>
    </BCardHeader>
    <BCardBody
      v-if="!noBodyBoolean"
      :overlay="overlay"
      :bg-variant="bodyBgVariant"
      :tag="bodyTag"
      :text-variant="bodyTextVariant"
      :subtitle="subtitle"
      :subtitle-tag="subtitleTag"
      :subtitle-text-variant="subtitleTextVariant"
      :title="title"
      :title-tag="titleTag"
      :class="bodyClass"
    >
      <slot>
        {{ bodyText }}
      </slot>
    </BCardBody>
    <slot v-else>
      {{ bodyText }}
    </slot>
    <BCardFooter
      v-if="footer || hasFooterSlot || footerHtml"
      :bg-variant="footerBgVariant"
      :border-variant="footerBorderVariant"
      :variant="footerVariant"
      :html="footerHtml"
      :tag="footerTag"
      :text-variant="footerTextVariant"
      :class="footerClass"
    >
      <slot name="footer">
        {{ footer }}
      </slot>
    </BCardFooter>
    <slot v-if="imgBottomBoolean" name="img">
      <BCardImg v-if="imgSrc" v-bind="imgAttr" />
    </slot>
  </component>
</template>

<script setup lang="ts">
import type {
  AlignmentTextHorizontal,
  Booleanish,
  ClassValue,
  ColorExtendables,
  ColorVariant,
  TextColorVariant,
} from '../../types'
import {isEmptySlot} from '../../utils'
import {computed, toRef, useSlots} from 'vue'
import {useBooleanish, useColorVariantClasses} from '../../composables'
import BCardImg from './BCardImg.vue'
import BCardHeader from './BCardHeader.vue'
import BCardBody from './BCardBody.vue'
import BCardFooter from './BCardFooter.vue'

const props = withDefaults(
  defineProps<
    {
      align?: AlignmentTextHorizontal
      bodyBgVariant?: ColorVariant | null
      bodyTextVariant?: TextColorVariant | null
      bodyClass?: ClassValue
      bodyTag?: string
      borderVariant?: ColorVariant | null
      footer?: string
      footerVariant?: ColorVariant | null
      footerBgVariant?: ColorVariant | null
      footerBorderVariant?: ColorVariant | null
      footerClass?: ClassValue
      footerHtml?: string
      footerTag?: string
      footerTextVariant?: TextColorVariant | null
      header?: string
      headerBgVariant?: ColorVariant | null
      headerBorderVariant?: ColorVariant | null
      headerClass?: ClassValue
      headerHtml?: string
      headerTag?: string
      headerVariant?: ColorVariant | null
      headerTextVariant?: TextColorVariant | null
      imgAlt?: string
      imgBottom?: Booleanish
      imgEnd?: Booleanish
      imgHeight?: string | number
      imgSrc?: string
      imgStart?: Booleanish
      imgTop?: Booleanish
      imgWidth?: string | number
      noBody?: Booleanish
      overlay?: Booleanish
      subtitle?: string
      subtitleTag?: string
      subtitleTextVariant?: TextColorVariant | null
      tag?: string
      title?: string
      titleTag?: string
      bodyText?: string
    } & ColorExtendables
  >(),
  {
    footerVariant: null,
    headerVariant: null,
    variant: null,
    align: undefined,
    bgVariant: null,
    bodyBgVariant: undefined,
    bodyClass: undefined,
    bodyTextVariant: undefined,
    borderVariant: null,
    footer: undefined,
    footerBgVariant: undefined,
    footerBorderVariant: undefined,
    footerClass: undefined,
    footerTextVariant: undefined,
    header: undefined,
    headerBgVariant: undefined,
    headerBorderVariant: undefined,
    headerClass: undefined,
    textVariant: null,
    headerTextVariant: undefined,
    imgAlt: undefined,
    title: undefined,
    subtitle: undefined,
    imgWidth: undefined,
    imgHeight: undefined,
    imgSrc: undefined,
    bodyTag: 'div',
    footerHtml: '',
    footerTag: 'div',
    headerHtml: '',
    headerTag: 'div',
    imgBottom: false,
    imgEnd: false,
    imgStart: false,
    bodyText: '',
    imgTop: false,
    noBody: false,
    overlay: false,
    subtitleTag: 'h6',
    subtitleTextVariant: 'body-secondary',
    tag: 'div',
    titleTag: 'h4',
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

const slots = useSlots()

const imgBottomBoolean = useBooleanish(() => props.imgBottom)
const imgEndBoolean = useBooleanish(() => props.imgEnd)
const imgStartBoolean = useBooleanish(() => props.imgStart)
const noBodyBoolean = useBooleanish(() => props.noBody)

const hasHeaderSlot = toRef(() => !isEmptySlot(slots.header))
const hasFooterSlot = toRef(() => !isEmptySlot(slots.footer))

const resolvedBackgroundClasses = useColorVariantClasses(props)

const computedClasses = computed(() => [
  resolvedBackgroundClasses.value,
  {
    [`text-${props.align}`]: props.align !== undefined,
    [`border-${props.borderVariant}`]: props.borderVariant !== null,
    'flex-row': imgStartBoolean.value,
    'flex-row-reverse': imgEndBoolean.value,
  },
])

const imgAttr = computed(() => ({
  src: props.imgSrc,
  alt: props.imgAlt,
  height: props.imgHeight,
  width: props.imgWidth,
  bottom: props.imgBottom,
  end: props.imgEnd,
  start: props.imgStart,
  top: props.imgTop,
}))
</script>
