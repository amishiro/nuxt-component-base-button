<template>
  <component
    :is="tag"
    :href="href"
    :to="to"
    class="base-button"
    :class="[addClassSize]"
    @click="$emit('click', $event)"
  >
    <div class="base-button__inner">
      <span class="base-button__slot"><slot /></span>
      <span class="base-button__icon">{{ icon }}</span>
    </div>
  </component>
</template>

<script>
export default {
  props: {
    // ancher用のアドレス
    // tag指定のためにあえてpropsへ取り込む
    href: {
      type: String,
      default: undefined,
    },

    // nuxt-link用のアドレス
    // tag指定のためにあえてpropsへ取り込む
    to: {
      type: String,
      default: undefined,
    },

    // アイコン指定
    icon: {
      type: String,
      default: '+',
    },

    // 横幅のサイズ
    size: {
      type: String,
      default: 'medium',
      validator(val) {
        return ['full', 'large', 'medium', 'small'].includes(val)
      },
    },
  },

  computed: {
    /**
     * propsに合わせてcomponentタグを生成
     * @returns { String } - nuxt-link || a || button
     */
    tag() {
      if (this.to) return 'nuxt-link'
      if (this.href) return 'a'
      return 'button'
    },

    /**
     * 横幅のサイズ用のclassを生成
     * @returns {( String | undefined )} - class名 || undefined
     */
    addClassSize() {
      return this.size ? `is-size-${this.size}` : undefined
    },
  },
}
</script>

<style lang="scss" scoped>
//-------------------- ベースマージンの設定

// $gap: 8px; //ベースに8pxは計算しやすい
// $gap-s: $gap * 2; //16px
// $gap-m: $gap * 3; //24px
// $gap-l: $gap * 5; //40px
// $gap-xl: $gap * 8; //64px

// 高さ指定用mixin
// 複数行のslotに対応(且つ、ie11対応)するため、calcとline-heightの設定で調整
// min-heightで書きたいけど、ie11で崩れるのでしかたなし
@mixin height {
  padding: calc((64px - 1.2em) / 2) $gap-m;
  line-height: 1.2;
}

// デザイン調整用mixin - 自由に変更する
@mixin details {
  color: black;
  border: 1px solid black;
  border-radius: 4px;
  transition: all 0.8s;
  &:hover {
    color: white;
    background-color: black;
    transition: all 0.3s;
  }
}

.base-button {
  position: relative;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  text-decoration: none;
  cursor: pointer;
  outline: none;
  @include height;
  @include details;

  &.is-size-full {
  }

  &.is-size-small {
    max-width: 200px;
  }

  &.is-size-medium {
    max-width: 320px;
  }

  &.is-size-large {
    max-width: 480px;
  }

  //.base-button__slot

  &__slot {
  }

  // .base-button__icon

  &__icon {
    position: absolute;
    top: 50%;
    right: $gap;
    transform: translateY(-50%);
  }
}
</style>
