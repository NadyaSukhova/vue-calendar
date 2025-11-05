<template>
  <div class="calendar">
    {{ lang == "Rus" ? 'Выбранная дата' : 'Selected date' }}: {{ todayText }}

    <div class="calendar-header">
      <button @click="prevMonth">◀</button>
      <span>{{ monthsList[chosenMonth] }} {{ chosenYear }}</span>
      <button @click="nextMonth">▶</button>
    </div>

<div class="week-days">
        <div v-for="day in weekList" :key="day" class="week-day">
          {{ day }}
        </div>
      </div>
    <div class="calendar-days">
      <div v-for="(day, index) in daysArray" :key="index" @click="selectDate(day)"
        :class="(day.isCurrentMonth != 'now' ? 'previous-days' : (isSelected(day.date) ? 'selected' : ''))">
        {{ day.number }}
      </div>
    </div>
  </div>

</template>

<script>
import { months } from '@/constants/months.js';
import { week } from '@/constants/week.js';
import '../assets/styles/calendar.css';

function DateToInput(today) {
  const year = today.getFullYear()
  const month = (today.getMonth() < 9 ? '0' : '') + String(today.getMonth() + 1)
  const day = (today.getDate() < 9 ? '0' : '') + String(today.getDate())
  return `${year}-${month}-${day}`
}

export default {
  name: 'Calendar',
  props: {
    lang: {
      type: String,
      default: 'Rus'
    }
  },
  data() {
    const today = new Date();
    return {
      today: today,
      week,
      months,
      chosenYear: today.getFullYear(),
      chosenMonth: today.getMonth(),
      chosenDay: today.getDate()
    }
  },
  computed: {

    monthsList() {
      return this.lang == 'Rus' ? months['Rus'] : months['En'];
    },
    weekList() {
      return this.lang == 'Rus' ? week['Rus'] : week['En'];
    },
    todayText(today) {
      const year = this.today.getFullYear()
      const month = (this.today.getMonth() < 9 ? '0' : '') + String(this.today.getMonth() + 1)
      const day = (this.today.getDate() < 9 ? '0' : '') + String(this.today.getDate())
      return `${day}.${month}.${year}`
    },
    daysArray() {
      const days = [];
      const daysInPrevMonth = new Date(this.chosenYear, this.chosenMonth, 0).getDate();
      var nextMonth = this.chosenMonth + 1;
      var nextOrThisYear = this.chosenYear;
      const firstDay = new Date(this.chosenYear, this.chosenMonth);
      const firstDayOfWeek = firstDay.getDay();
      const lastDay = new Date(nextOrThisYear, nextMonth, 0);

      for (let i = firstDayOfWeek - 1; i >= 0; i--) {
        days.push({
          date: new Date(this.chosenYear, this.chosenMonth - 1, daysInPrevMonth - i),
          number: daysInPrevMonth - i,
          isCurrentMonth: 'prev'
        });
      }
      for (let i = 1; i <= lastDay.getDate(); i++) {

        days.push({
          date: new Date(this.chosenYear, this.chosenMonth, i),
          number: i,
          isCurrentMonth: 'now'
        });
      }

      const remainingDays = 42 - days.length;
      if (nextMonth == 12) {
        nextMonth = 0;
        nextOrThisYear += 1;
      }
      for (let i = 1; i <= remainingDays; i++) {
        days.push({
          date: new Date(nextOrThisYear, nextMonth, i),
          number: i,
          isCurrentMonth: 'next'
        });
      }

      return days;
    }
  },
  methods: {
    prevMonth() {
      this.chosenMonth -= 1;
      if (this.chosenMonth == -1) {
        this.chosenMonth = 11;
        this.chosenYear -= 1;
      }
    },
    nextMonth() {
      this.chosenMonth += 1;
      if (this.chosenMonth == 12) {
        this.chosenMonth = 0;
        this.chosenYear += 1;
      }
    },
    selectDate(day) {
      if (day.isCurrentMonth == 'prev') {
        this.prevMonth();
      }
      if (day.isCurrentMonth == 'next') {
        this.nextMonth();
      }
      this.today = new Date(day.date);
      this.todayInput = DateToInput(this.today);
    },
    isSelected(day) {
      return new Date(day).toDateString() === this.today.toDateString();
    }
  },
  watch: {
    todayInput(newValue) {
      this.today = new Date(newValue);
      const year = newValue.getFullYear();
      const month = (newValue.getMonth() < 9 ? '0' : '') + String(newValue.getMonth() + 1);
      const day = (newValue.getDate() < 9 ? '0' : '') + String(newValue.getDate());
      return `${year}-${month}-${day}`;
    },
    lang: {
      handler() {
        this.$forceUpdate();
      },
      immediate: true
    }
  }
}
</script>