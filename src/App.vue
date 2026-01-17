<template>
  <div class="app">
    <!--顶部固定导航栏-->
    <header>
      <div class="userinfo">
        <img src="./assets/avatar.png" alt="avatar">
        <h1>TokyoBoy0701 さん</h1>
      </div>
      <div class="menu">
        <img src="./assets/menu.svg" alt="menu">
      </div>
    </header>

    <!--PC端布局-->
    <div class="pc_layout" v-if="windowWidth >= 880">
      <div class="sec1">
        <img src="./assets/back_top.jpg" alt="">
      </div>
      <div class="sec2">
        <img class="back_bottom_left" src="./assets/back_bottom_left.jpg" alt="">

        <!--预约容器组件-->
        <BookContainer class="book_container" />
      </div>
    </div>

    <!--移动端布局-->
    <div class="mobile_layout" v-else>
      <div class="sec3">
        <img src="./assets/mobile_back.jpg" alt="">

        <!--预约容器组件-->
        <BookContainer class="book_container_mobile" />
      </div>
    </div>


    <!--委托列表-->
    <main>
      <div class="order_container">
        <!--委托标题-->
        <div class="order_title">
          <h1>みんなの依頼：</h1>
        </div>

        <!--委托列表-->
        <div class="order_list">
          <div class="order_card" v-for="booking in bookings" :key="booking.id">
            <h2>{{ booking.nickname }}<span>の委託：</span></h2>
            <p>{{ booking.reason }}</p>
            <div class="date_place">
              <span>{{ booking.datetime || '選ばれなかった' }}</span>
              <span>{{ booking.location || '未記入' }}</span>
              <button @click="cancelBooking(booking.id)">キャンセル</button>
            </div>
          </div>
          <div class="order_card" v-if="bookings.length === 0">
            <h2><span>委託はありません</span></h2>
            <p>まだ依頼の記録はありません</p>
            <div class="date_place">
              <span></span>
              <span></span>
              <button style="display:none"></button>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch } from 'vue'
import BookContainer from './components/BookContainer.vue'

const windowWidth = ref(window.innerWidth)

interface BookingData {
  id: string
  datetime: string
  location: string
  nickname: string
  reason: string
  price: string
  createdAt: string
}

const bookings = ref<BookingData[]>([])

const loadBookings = () => {
  const stored = localStorage.getItem('bookings')
  if (stored) {
    bookings.value = JSON.parse(stored)
  }
}

const cancelBooking = (id: string) => {
  if (confirm('この依頼を本当にキャンセルしますか？')) {
    bookings.value = bookings.value.filter(b => b.id !== id)
    localStorage.setItem('bookings', JSON.stringify(bookings.value))
  }
}

const handleResize = () => {
  windowWidth.value = window.innerWidth
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
  loadBookings()
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})

watch(() => localStorage.getItem('bookings'), () => {
  loadBookings()
})
</script>

<style scoped lang="less">
page {
  width: 100vw;
  background: #fff;
}

.app {
  width: 100vw;
  background: #fff;
  display: grid;
  justify-items: center;
}

// 顶部固定导航栏
header {
  width: 100vw;
  height: 60px;
  background: #b1d6eb;
  box-shadow: 0 5px 15px rgba(49, 78, 83, 0.15);
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  z-index: 999;
  position: fixed;
  top: 0;

  .userinfo {
    display: flex;
    flex-direction: row;
    justify-content: left;
    align-items: center;
    gap: 13px;
    padding-left: 25px;

    img {
      width: 38px;
      height: 38px;
      border-radius: 50%;
    }

    h1 {
      font-size: 22px;
      font-weight: 500;
      color: #fff;
    }
  }

  .menu {
    display: flex;
    flex-direction: row;
    justify-content: right;
    align-items: center;
    gap: 13px;
    padding-right: 25px;

    img {
      width: 32px;
      height: 32px;
      cursor: pointer;
    }
  }
}

// PC端布局
.pc_layout {
  width: 70vw;
  min-width: 800px;
  display: grid;
  justify-items: center;
  padding-top: 60px;
  z-index: 0;

  .sec1 {
    width: 100%;
    height: calc(100vh - 50vh - 60px);
    display: grid;
    justify-items: center;
    align-items: center;

    img {
      width: 100%;
      height: calc(100vh - 50vh - 60px);
    }
  }

  .sec2 {
    width: 100%;
    height: calc(100vh - 50vh - 60px);
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;

    .back_bottom_left {
      width: 45%;
      min-width: 400px;
      height: calc(100vh - 50vh - 60px);
      display: grid;
      justify-items: center;
      align-items: center;
    }

    .book_container {
      width: calc(45% - 40px);
      min-width: 400px;
      height: calc(100vh - 50vh - 100px);
      display: grid;
      justify-items: center;
      align-items: center;
    }
  }
}

// 移动端布局
.mobile_layout {
  width: 100vw;
  height: auto;
  display: grid;
  justify-items: center;
  align-items: center;

  .sec3 {
    width: 100%;
    height: calc(100vh - 20vh - 60px);
    display: grid;
    justify-items: center;
    align-items: center;
    padding-top: 30px;

    img {
      width: 100%;
      height: auto;
    }

    .book_container {
      width: 100vw;
    }
  }
}


// 委托列表
main {
  width: 100vw;
  display: grid;
  margin-top: 100px;
  padding-bottom: 100px;
  justify-items: center;

  .order_container {
    width: calc(900px - 36px);
    height: auto;
    display: grid;
    justify-items: center;
    align-items: center;
    background: #f8e7c6;
    gap: 13px;
    padding: 22px 18px 16px 18px;
    border-radius: 13px;

    // 委托标题
    .order_title {
      width: 100%;
      padding: 5px 0 13px 0;
      border-bottom: 1px solid #ccc;

      h1 {
        font-size: 22px;
        font-weight: 500;
      }
    }

    // 委托列表
    .order_list {
      width: 100%;
      height: auto;
      display: grid;
      justify-items: center;
      align-items: center;
      gap: 13px;

      .order_card {
        width: 100%;
        display: grid;
        justify-items: left;
        align-items: center;
        gap: 3px;
        padding-bottom: 15px;
        border-bottom: 1px solid #ccc;

        &:hover {
          .date_place button {
            display: block;
          }
        }

        h2 {
          font-size: 19px;
          font-weight: bold;

          span {
            font-size: 19px;
            font-weight: bold;
          }
        }

        p {
          font-size: 17px;
          font-weight: normal;
        }

        .date_place {
          display: flex;
          flex-direction: row;
          justify-content: left;
          align-items: center;
          gap: 30px;

          span {
            font-size: 15px;
            font-weight: normal;
            color: #666;
          }

          button {
            border: none;
            color: #666;
            font-size: 15px;
            cursor: pointer;
            display: none;

            &:hover {
              color: #bd1b1b;
            }
          }
        }
      }

      .order_card:last-child {
        border-bottom: none;
      }
    }
  }
}


@media screen and (max-width: 800px) {
  main {
    width: 100vw;
    display: grid;
    justify-items: center;
    margin-top: 250px;

    .order_container {
      border-radius: 0;
      width: calc(100vw - 36px);

      .date_place {
        gap: 13px !important;

        span {
          max-width: 120px;
          font-size: 14px !important;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }

        button {
          font-size: 14px !important;
        }
      }
    }
  }
}
</style>
