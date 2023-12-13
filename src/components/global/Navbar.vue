<script>

export default {
  props: {
    show: false,
    isMobile: false,
  }
}
</script>

<template>
  <nav v-if="isMobile">
    <div v-show="show" class="menu_body mobile">
      <ul class="menu_list">
        <li class="menu_item"><router-link to="/" class="menu_link"></router-link></li>
        <li class="menu_item"><router-link  to="/catalog" class="menu_link">Каталог</router-link></li>
        <li class="menu_item"><router-link to="/credit" class="menu_link">Кредит</router-link></li>
        <li class="menu_item"><router-link to="/about" class="menu_link">О компании</router-link></li>
        <li class="menu_item"><router-link to="/contacts" class="menu_link">Контакты</router-link></li>
      </ul>
    </div>
  </nav>
  <nav v-else>
    <div class="menu">
      <ul class="menu_list">
        <li class="menu_item"><router-link to="/" class="menu_link"></router-link></li>
        <li class="menu_item"><router-link  to="/catalog" class="menu_link">Каталог</router-link></li>
        <li class="menu_item"><router-link to="/credit" class="menu_link">Кредит</router-link></li>
        <li class="menu_item"><router-link to="/about" class="menu_link">О компании</router-link></li>
        <li class="menu_item"><router-link to="/contacts" class="menu_link">Контакты</router-link></li>
      </ul>
    </div>
  </nav>
</template>

<style scoped lang="scss">
@media (min-width: 992px) {
  .menu_list {
    display: flex;
    align-items: center;

    &>li {
      padding: 10px 0px;
    }
  }
}

@media (max-width: 992px) {
  .menu_icon {
    z-index: 5;
    display: block;
    position: relative;
    width: 30px;
    height: 18px;
    cursor: pointer;

    & span,
    &::before,
    &::after {
      left: 0;
      position: absolute;
      width: 100%;
      height: 10%;
      transition: all .3s ease;
      background-color: #B40D16;
    }

    &::before,
    &::after {
      content: '';
    }

    &::before {
      top: 0;
    }

    &::after {
      bottom: 0;
    }

    & span {
      top: 50%;
      transform: scale(1) translate(0px, -50%);
    }

    &._active span {
      transform: scale(0) translate(0px, -50%);
    }

    &._active::before {
      top: 50%;
      transform: rotate(-45deg) translate(0px, -50%);
    }

    &._active::after {
      bottom: 50%;
      transform: rotate(45deg) translate(0px, 50%);
    }
  }
.menu_body {
  overflow: auto;
  position: fixed;
  top: 0;
  right: -100%;
  width: 100%;
  height: 100vh;
  padding: 100px 30px;
  transition: right .3s ease;
  background: #fff;
  backdrop-filter: blur(4px);

  @media (max-height: 500px) {
    height: 100dvh;
  }

  &::before {
    content: '';
    position: fixed;
    top: 0;
    right: 0;
    z-index: 2;
    height: 70px;
    width: 100%;
    backdrop-filter: blur(4px);
  }

  &.mobile {
    right: 0;

    @media (max-width: 574.98px) {
      display: flex;
      flex-direction: column;
    }
  }

  .menu_list>li {
    flex-wrap: wrap;

    &:last-child {
      margin: 0;
    }

    &:not(:last-child) {
      margin: 0px 0px 24px 0px;
    }
  }
}
</style>