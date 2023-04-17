<template>
  <header class="top">
    <div class="logo">
      <div class="logo__icon">
        <svg
          width="41"
          height="34"
          viewBox="0 0 41 34"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <rect
            x="1.5"
            y="2"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="14.5"
            y="2"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="27.5"
            y="2"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="1.5"
            y="14"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="14.5"
            y="14"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="27.5"
            y="14"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="1.5"
            y="26"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="14.5"
            y="26"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
          <rect
            x="27.5"
            y="26"
            width="12"
            height="6"
            stroke="#006CFE"
            stroke-width="3"
          />
        </svg>
      </div>
      <div class="logo__text">Система<br />таблиц</div>
    </div>
    <div class="user">
      <div class="user__text">Светлана<br />Сотрудник</div>
      <div class="user__img">
        <img alt="Светлана" src="../assets/user.png" />
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderTop",
};
</script>

<style lang="scss">
@import "../styles/style";

.top {
  display: flex;
  justify-content: space-between;
  padding: 20px 60px;
  background-color: $color-white;

  @media screen and (max-width: 1366px) {
    padding: 20px 40px;
  }

  @media screen and (max-width: 767px) {
    padding: 20px 12px;
  }

  .logo {
    display: flex;
    align-items: center;

    .logo__icon {
      font-size: 0;
      margin-right: 10px;
    }

    .logo__text {
      font-weight: 800;
      font-size: 16px;
      line-height: 18px;
      color: $color-blue;
      text-transform: uppercase;
    }
  }

  .user {
    display: flex;
    .user__text {
      color: $color-medium;

      &:first-line {
        color: $color-blue;
      }
    }
    .user__img {
      width: 44px;
      height: 44px;
      margin-left: 8px;
    }
  }
}
</style>
