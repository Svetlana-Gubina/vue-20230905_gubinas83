<template>
  <div class="calendar-view">
    <div class="calendar-view__controls">
      <div class="calendar-view__controls-inner">
<<<<<<< HEAD
        <button @click.stop="previousMonth" class="calendar-view__control-left" type="button" aria-label="Previous month"></button>
        <div class="calendar-view__date">{{ date.toLocaleDateString("en-EN", {
          month: 'long',
          year: 'numeric',
        }) }} </div>
        <button @click.stop="nextMonth" class="calendar-view__control-right" type="button" aria-label="Next month"></button>
=======
        <button
          class="calendar-view__control-left"
          type="button"
          aria-label="Previous month"
          @click.stop="setPreviousMonth"
        ></button>
        <div class="calendar-view__date">{{ localDate }}</div>
        <button
          class="calendar-view__control-right"
          type="button"
          aria-label="Next month"
          @click.stop="setNextMonth"
        ></button>
>>>>>>> 7abebd12c05a3c79a93937837e1582a7c4f5e4ea
      </div>
    </div>

    <div class="calendar-view__grid">
<<<<<<< HEAD
      <div v-for="prevDay in previousDays"  :key="prevDay" class="calendar-view__cell calendar-view__cell_inactive" tabindex="0">
        <div class="calendar-view__cell-day">{{ prevDay }}</div>
        <div class="calendar-view__cell-content"></div>
      </div>


      <div v-for="currentDay in currentMonthDays" :key="currentDay" class="calendar-view__cell" tabindex="0">
        <div class="calendar-view__cell-day">{{ currentDay }}</div>
        <div class="calendar-view__cell-content">
          <a v-for="meetup in meetups.filter((meetup) => new Date(meetup.date).getDate() === currentDay && new Date(meetup.date).getMonth() === date.getMonth() && new Date(meetup.date).getFullYear() === date.getFullYear())" :key="meetup.id" :href="`/meetups/${meetup.id}`" class="calendar-event">{{ meetup.title }}</a>

        </div>
      </div>


      <div v-for="nextDay in nextDays" :key="nextDay" class="calendar-view__cell calendar-view__cell_inactive" tabindex="0">
        <div class="calendar-view__cell-day">{{ nextDay }}</div>
        <div class="calendar-view__cell-content"></div>
      </div>
=======
      <div
        v-for="cell in calendarCells"
        :key="cell.timestamp"
        class="calendar-view__cell"
        :class="{ 'calendar-view__cell_inactive': !cell.isCurrentMonth }"
        :aria-label="cell.localDateString"
        tabindex="0"
      >
        <div class="calendar-view__cell-day">{{ cell.date }}</div>
        <div class="calendar-view__cell-content">
          <a
            v-for="meetup in meetupsByDate[cell.timestamp]"
            :key="meetup.id"
            :href="`/meetups/${meetup.id}`"
            class="calendar-event"
          >
            {{ meetup.title }}
          </a>
        </div>
      </div>
>>>>>>> 7abebd12c05a3c79a93937837e1582a7c4f5e4ea
    </div>
  </div>
</template>

<script>
import { addDays, addMonths, getFirstDateOfMonth, getLastDateOfMonth, getWeekday } from '../utils/dateUtils.js';

export default {
  name: 'MeetupsCalendar',

  props: {
    meetups: {
      type: Array,
      required: true,
    },
  },
<<<<<<< HEAD
  data() {
    return {
      date: new Date(),
      monthes: [
        'Январь',
        'Февраль',
        'Март',
        'Апрель',
        'Май',
        'Июнь',
        'Июль',
        'Август',
        'Сентябрь',
        'Ноябрь',
        'Декабрь',
      ],
    }
  },
  methods: {
    previousMonth() {
      this.date = new Date(this.date.setMonth(this.date.getMonth() - 1));
    },
    nextMonth() {
      this.date = new Date(this.date.setMonth(this.date.getMonth() + 1));
    },
  },
  computed: {
    previousDays() {
      let arr = [];
      let firstWeekDay = new Date(this.date.getFullYear(), this.date.getMonth(), 1).getDay() - 1;
      if(firstWeekDay === -1) {
        firstWeekDay = 6;
      }

      let previousMonthLastDate = new Date(this.date.getFullYear(), this.date.getMonth() , 0).getDate();
      let count = firstWeekDay;
      for(let i = previousMonthLastDate; i > previousMonthLastDate - count; i--) {
        arr.push(i);
      }

      return arr.reverse();
    },
    nextDays() {
      let arr = [];
      let lastWeekDay = new Date(this.date.getFullYear(), this.date.getMonth() + 1, 0).getDay() - 1;
      if(lastWeekDay === -1) {
        lastWeekDay = 6;
      }
      let count = 6 - lastWeekDay + 1;
      for(let i = 1; i < count; i++) {
        arr.push(i);
      }
      return arr;
    },
    currentMonthDays() {
      let arr = [];
      let count = new Date(this.date.getFullYear(), this.date.getMonth() + 1, 0).getDate();
      for(let i = 1; i <= count; i++) {
        arr.push(i);
      }
      return arr;
    }
  }

=======

  data() {
    return {
      currentDate: getFirstDateOfMonth(new Date()),
    };
  },

  computed: {
    localDate() {
      return this.currentDate.toLocaleDateString(navigator.language, {
        month: 'long',
        year: 'numeric',
      });
    },

    calendarCells() {
      const lastDateOfMonth = getLastDateOfMonth(this.currentDate);
      const startDate = addDays(this.currentDate, -(getWeekday(this.currentDate) - 1));
      const finishDate = addDays(lastDateOfMonth, 7 - getWeekday(lastDateOfMonth));

      const cells = [];

      for (let dayOfCalendar = startDate; dayOfCalendar <= finishDate; dayOfCalendar = addDays(dayOfCalendar, 1)) {
        cells.push({
          timestamp: +dayOfCalendar,
          year: dayOfCalendar.getUTCFullYear(),
          month: dayOfCalendar.getUTCMonth(),
          date: dayOfCalendar.getUTCDate(),
          isCurrentMonth: dayOfCalendar.getUTCMonth() === this.currentDate.getUTCMonth(),
          localDateString: dayOfCalendar.toLocaleDateString(navigator.language, { dateStyle: 'long' }),
        });
      }

      return cells;
    },

    meetupsByDate() {
      const result = {};
      for (const meetup of this.meetups) {
        if (!result[meetup.date]) {
          result[meetup.date] = [meetup];
        } else {
          result[meetup.date].push(meetup);
        }
      }
      return result;
    },
  },

  methods: {
    setPreviousMonth() {
      this.currentDate = addMonths(this.currentDate, -1);
    },

    setNextMonth() {
      this.currentDate = addMonths(this.currentDate, 1);
    },
  },
>>>>>>> 7abebd12c05a3c79a93937837e1582a7c4f5e4ea
};
</script>

<style scoped>
.calendar-view {
}

.calendar-view__controls {
  text-align: center;
  font-weight: 700;
  font-size: 24px;
  line-height: 1;
  color: var(--blue);
  background-color: var(--blue-extra);
  padding: 24px;
  display: flex;
  justify-content: center;
}

.calendar-view__controls-inner {
  max-width: 325px;
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  text-transform: capitalize;
}

.calendar-view__controls-inner button {
  border: none;
  padding: 0;
}

.calendar-view__control-left,
.calendar-view__control-right {
  width: 30px;
  height: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  cursor: pointer;
  transition: 0.3s all;
  background: url('@/assets/icons/icon-pill-active.svg') left center no-repeat;
  background-size: cover;
}

.calendar-view__control-left:hover,
.calendar-view__control-right:hover {
  opacity: 0.8;
}

.calendar-view__control-right {
  transform: rotate(180deg);
}

.calendar-view__grid {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
}

.calendar-view__grid {
  border: 1px solid var(--grey);
  border-bottom: none;
}

.calendar-view__cell {
  position: relative;
  height: auto;
  padding: 6px 8px;
  background-color: var(--white);
  color: var(--grey-8);
  font-weight: 400;
  font-size: 16px;
  line-height: 24px;
  border-bottom: 1px solid var(--grey);
  border-left: 1px solid var(--grey);
  text-align: right;
}

.calendar-view__cell.calendar-view__cell_inactive {
  background-color: var(--grey-light);
}

@media all and (max-width: 767px) {
  .calendar-view__cell:nth-child(5n + 1) {
    border-left: none;
  }
}

@media all and (min-width: 767px) {
  .calendar-view__grid {
    grid-template-columns: repeat(7, 1fr);
  }

  .calendar-view__cell {
    height: 144px;
  }

  .calendar-view__cell:nth-child(7n + 1) {
    border-left: none;
  }
}

.calendar-event {
  --max-lines: 2;
  --line-height: 16px;

  display: block;
  text-align: left;
  text-decoration: none;
  text-overflow: ellipsis;
  overflow: hidden;
  font-size: 14px;
  font-weight: 600;
  line-height: var(--line-height);
  color: var(--white);
  padding: 4px 6px;
  border-radius: 2px;
  background-color: var(--blue);
  margin-top: 4px;
}

@media all and (min-width: 767px) {
  .calendar-event {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    max-height: calc(var(--max-lines) * var(--line-height) + 6px);
  }
}
</style>
