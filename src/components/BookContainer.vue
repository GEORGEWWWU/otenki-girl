<template>
    <form @submit.prevent="addToCart">
        <div class="app_book">
            <div class="book_item book_price">
                <h1>¥ 3,400<span>（税込）</span></h1>
            </div>
            <div class="book_item">
                <p>雨でお困りのあなたに、100%の晴れ女がおひさまを連れてきます😆もどんな場…</p>
            </div>
            <div class="book_item book_btn">
                <button type="submit">
                    <img src="../assets/购物车.svg" alt="">
                    <p>カートに入れる</p>
                </button>
            </div>
            <div class="book_item bookform_top">
                <div class="input_box">
                    <p>ご希望日時</p>
                    <select v-model="formData.datetime" required>
                        <option value="">選択して下さい</option>
                        <option v-for="option in datetimeOptions" :key="option.value" :value="option.value">{{
                            option.label }}</option>
                    </select>
                </div>
                <div class="input_box">
                    <p>晴れにしたい場所</p>
                    <input type="text" v-model="formData.location">
                </div>
            </div>
            <div class="book_item">
                <div class="input_box">
                    <p>ニックネーム <span>※必須</span></p>
                    <input type="text" v-model="formData.nickname" required>
                </div>
            </div>
            <div class="book_item">
                <div class="input_box">
                    <p>晴れにしたい理由 <span>※必須</span></p>
                    <textarea type="text" v-model="formData.reason" required placeholder="できるだけ詳しくお書きください"></textarea>
                </div>
            </div>
        </div>
    </form>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface BookingData {
    id: string
    datetime: string
    location: string
    nickname: string
    reason: string
    price: string
    createdAt: string
}

const formData = ref({
    datetime: '',
    location: '',
    nickname: '',
    reason: ''
})

const datetimeOptions = computed(() => {
    const options: { value: string, label: string }[] = []
    const today = new Date()
    const times = ['10:00', '14:00']

    for (let i = 0; i < 3; i++) {
        const date = new Date(today)
        date.setDate(today.getDate() + i)

        const year = date.getFullYear()
        const month = String(date.getMonth() + 1).padStart(2, '0')
        const day = String(date.getDate()).padStart(2, '0')
        const dateStr = `${year}-${month}-${day}`

        times.forEach(time => {
            options.push({
                value: `${dateStr} ${time}`,
                label: `${dateStr} ${time}`
            })
        })
    }

    return options
})

const addToCart = () => {
    if (!formData.value.nickname || !formData.value.reason) {
        alert('必須項目を入力してください')
        return
    }

    const booking: BookingData = {
        id: Date.now().toString(),
        datetime: formData.value.datetime,
        location: formData.value.location,
        nickname: formData.value.nickname,
        reason: formData.value.reason,
        price: '3,400',
        createdAt: new Date().toISOString()
    }

    const existingBookings = JSON.parse(localStorage.getItem('bookings') || '[]')
    existingBookings.push(booking)
    localStorage.setItem('bookings', JSON.stringify(existingBookings))

    alert('カートに追加しました！')

    formData.value = {
        datetime: '',
        location: '',
        nickname: '',
        reason: ''
    }

    window.location.reload()
}
</script>

<style scoped lang="less">
.app_book {
    width: 100%;
    height: 100%;
    display: grid;
    justify-items: center;
    align-items: center;
    background: #f8e7c6;
    border-radius: 13px;
    padding: 20px;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.3);

    .book_item {
        width: 100%;

        p {
            font-size: 20px;
            font-weight: bold;
        }
    }

    .book_price {
        display: flex;
        flex-direction: row;
        justify-content: left;
        align-items: center;
        gap: 4px;

        h1 {
            font-size: 28px;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            font-weight: normal;
            color: #bd1b1b;

            span {
                font-size: 18px;
                font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
                font-weight: normal;
                color: #bd1b1b;
            }
        }
    }

    .book_btn {
        width: 100%;
        display: grid;
        justify-items: center;
        align-items: center;
        padding: 5px 0;

        button {
            width: 70%;
            height: 40px;
            background: #bd1b1b;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            display: flex;
            justify-content: left;
            align-items: center;
            transition: all 0.15s ease-in-out;

            &:hover {
                background: #9a1717;
            }

            &:active {
                transform: scale(0.98);
            }

            img {
                width: 24px;
                height: 24px;
                padding: 0 20px;
                border-right: 1px solid #fff;
            }

            p {
                width: calc(100% - 44px);
                font-size: 20px;
                font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
                font-weight: normal;
                color: #fff;
            }
        }
    }

    .bookform_top {
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        gap: 13px;
    }

    .input_box {
        width: 100%;
        display: grid;
        gap: 2px;
        margin: auto 0;

        p {
            font-size: 15px;
            font-weight: bolder;

            span {
                font-size: 12px;
                font-weight: bolder;
                color: #bd1b1b;
            }
        }

        input {
            width: calc(100% - 24px);
            height: 25px;
            padding: 2px 10px;
            border: none;
            background: #fff;
        }

        textarea {
            resize: vertical;
            width: calc(100% - 24px);
            min-height: 25px;
            padding: 2px 10px;
            border: none;
            background: #fff;
            outline: none;
        }

        select {
            width: calc(100% - 24px);
            height: 30px;
            padding: 2px 10px;
            border: none;
            background: #fff;
            outline: none;
        }
    }
}


@media screen and (max-width: 880px) {
    .app_book {
        width: calc(95% - 36px);
        margin: 0 auto;
        gap: 2px;
        padding: 18px;
    }
}
</style>
