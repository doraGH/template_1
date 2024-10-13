<template>
  <div class="p-kanban">
    <h2 class="p-kanban__text">
      <span class="en">About Us</span>
      <span class="zh">公司簡介</span>
    </h2>
    <div class="pic">
      <source srcset="@/assets/images/ban_about_s.jpg" media="(max-width: 767px)">
      <img src="@/assets/images/ban_about.jpg" alt="公司簡介" class="bg">
    </div>
  </div>
  <RouterView />
</template>

<style lang="scss" scoped>
  .p-kanban {
    position: relative;
    height: 400px;
    overflow: hidden;
    @media (max-width: 991px) {
      height: 300px;
    }
    @media (max-width: 639px) {
      height: 250px;
    }
    .pic {
      width: 100%;
      height: 100%;
    }
    .bg {
      display: block;
      position: absolute;
      width: auto;
      height: 100%;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
    }
    &__text {
      position: absolute;
      width: 100%;
      color: #fff;
      padding: 0 15px;
      text-align: center;
      top: 50%;
      margin-top: 50px;
      z-index: 1;
      transform: translateY(-50%);
      @media (max-width: 991px) {
        margin-top: 0px;
      }
      .en {
        display: block;
        line-height: 60px;
        font-size: 2.75rem;
        font-weight: bold;
        margin: 0;
        text-transform: uppercase;
        @media (max-width: 991px) {
          line-height: 40px;
          font-size: 2rem;
        }
      }
      .zh {
        display: block;
        line-height: 30px;
        font-size: 1.25rem;
        font-weight: bold;
        margin: 0;
        @media (max-width: 991px) {
          font-size: 1.125rem;
        }
      }
    }
  }

</style>
