<style lang="scss">
/* Theme Material Design
Red, Pink, Purple, Deep_Purple, Indigo, Blue, Light_Blue, Cyan, Teal, Green,
Light_Green, Lime, Yellow, Amber, Orange, Deep_Orange, Brown, Grey.
*/
$theme: 'Yellow';
@import '../../modules/colors';
@import 'animations';

  $day-size: 3.5rem;
  $day-margin: 1.4rem;
  $datepicker-width: ($day-size * 7) + ($day-margin * 2);
  $header-height: 8rem;

  .datepicker {
    position: absolute;
    width: $datepicker-width;
    font-size: 1.6rem;
    top: 100%;
    z-index: 5;
    background-color: $color-50;
    box-shadow: 0 1.4rem 4.5rem rgba(0,0,0,0.25), 0 1rem 1.8rem rgba(0,0,0,0.22);
  }
  .datepicker__header {
    background-color: $color-700;
    color: white;
    padding: $day-margin 1rem 0 2rem;
    height: $header-height;
  }
  .datepicker__year {
    position: relative;
    overflow: hidden;
    opacity: 0.8;
    height: 2rem;
    margin-bottom: .5rem;
    line-height: 2rem
  }
  .datepicker__date {
    position: relative;
    overflow: hidden;
    height: 3.2rem;
    font-size: 3.2rem;
    line-height: 3.2rem;
  }
  .datepicker__week {
    line-height: 1rem;
    font-weight: bold;
    color: rgba(0,0,0,0.8);
    padding: 0 $day-margin;
  }
  .datepicker__weekday {
    float: left;
    width: $day-size;
    text-align: center;
  }
  .datepicker__days {
    position: relative;
    overflow: hidden;
    margin: $day-margin $day-margin 0;
    height: $day-size * 5;
    transition: height 0.3s;
    &.has-6-weeks {
      height: $day-size * 6;
    }
    &.has-4-weeks {
      height: $day-size * 4;
    }
  }
  .datepicker__day {
    position: relative;
    width: $day-size;
    height: $day-size;
    float: left;
    text-align: center;
    line-height: $day-size;
    cursor: pointer;
    transition: color 450ms cubic-bezier(0.23, 1, 0.32, 1);
  }
  .datepicker__day__effect {
    position: absolute;
    top: 2px;
    left: 2px;
    width: 3rem;
    height: 3rem;
    border-radius: 50%;
    background-color: $color-600;
    transition: all 450ms cubic-bezier(0.23, 1, 0.32, 1);
    transform: scale(0);
    opacity: 0;
  }
  .datepicker__day__text {
    position: relative;
  }
  .datepicker__day:hover {
    color: white;
    .datepicker__day__effect {
      transform: scale(1);
      opacity: 0.6;
    }
  }
  .datepicker__day.selected {
    color: white;
    .datepicker__day__effect {
      transform: scale(1);
      opacity: 1;
    }
  }
  .datepicker__controls {
    position: relative;
    height: 5.6rem;
    line-height: 5.6rem;
    text-align: center;
    button {
      position: relative;
      border: none;
      background-color: transparent;
      user-select: none;
      outline: none;
      cursor: pointer;
      width: 5.6rem;
      height: 5.6rem;
    }
    svg {
      width: 2.4rem;
      height: 2.4rem;
      fill: rgba(0,0,0,0.87);
      vertical-align: middle;
    }
  }
  .datepicker__next {
    float: right;
  }
  .datepicker__nextm {
    float: right;
    left: 2.5rem;
  }
  .datepicker__prev {
    float: left;
  }
  .datepicker__prevm {
    left: -2.5rem;
    float: left;
  }
  .datepicker__month {
    position: absolute;
    height: 1.6rem;
    line-height: 1.6rem;
    overflow: hidden;
    top: 2rem;
    left: 0;
    right: 0;
    bottom: 0;
  }
  .datepicker__actions {
    text-align: right;
    padding: 0.8rem;
  }
  .datepicker__actions button {
    border: none;
    background-color: transparent;
    display: inline-block;
    cursor: pointer;
    outline: none;
    color: $color-700;
    font-weight: bold;
    font-size: 1.6rem;
    text-transform: uppercase;
    font-weight: 500;
    min-width: 7rem;
    line-height: 3rem;
    text-align: center;
    -webkit-appearance: none;
    transition: all 0.3s;
    &:hover {
      background-color: rgba(153, 153, 153, 0.2);
    }
  }
  .datepicker-slide-transition {
    opacity: 1;
    transition: all 0.3s;
    transform: translateY(0);
  }
  .datepicker-slide-leave, .datepicker-slide-enter {
    opacity: 0;
    transform: translateY(-1.5rem);
  }

</style>

<template>
  <div class="datepicker" v-if="visible" transition="datepicker-slide" @click.stop>
    <div class="datepicker__header">
      <div class="datepicker__year">
        <span v-for="year in [year]" transition="slideh" :class="dayDirection">{{ year }}</span>
      </div>
      <div class="datepicker__date">
        <span v-for="date_formatted in [date_formatted]" transition="slideh" :class="dayDirection">{{ date_formatted }}</span>
      </div>
    </div>
    <div class="datepicker__controls">
      <div class="datepicker__month">
        <span v-for="month in [month]" transition="slideh" :class="classDirection">{{ month.getFormatted() }}</span>
      </div>
      <button class="datepicker__next" @click="nextYear()">
        <svg viewBox="0 0 24 24">
          <path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"></path>
          <path d="M6 6 4.59 7.41 9.17 12 4.59 16.59 6 18 12 12 z"></path>
        </svg>
      </button>
      <button class="datepicker__nextm" @click="nextMonth()">
        <svg viewBox="0 0 24 24"><path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z"></path></svg>
      </button>
      <button class="datepicker__prev" @click='prevYear()'>
        <svg viewBox="0 0 24 24">
          <path d="M15.41 7.41L14 6l-6 6 6 6 1.41-1.41L10.83 12z"></path>
          <path d="M 19.41 7.41 18 6 12 12 18 18 19.41 16.59 14.83 12 z"></path>
        </svg>
      </button>
      <button class="datepicker__prevm" @click='prevMonth()'>
        <svg viewBox="0 0 24 24"><path d="M15.41 7.41L14 6l-6 6 6 6 1.41-1.41L10.83 12z"></path></svg>
      </button>
    </div>
    <div class="datepicker__week">
      <div v-for="day in days" track-by="$index" class="datepicker__weekday">
        {{ day }}
      </div>
    </div>
    <div class="datepicker__days" :class="classWeeks">
      <div v-for="month in [month]" transition="slidev" :class="classDirection">
        <div class="datepicker__day" v-bind:style="{width: (month.getWeekStart() * 3.5) + 'rem'}">
        </div>
        <div class="datepicker__day" @click="selectDate(day)" v-for="day in month.getDays()" :class="{selected: isSelected(day)}">
          <span class="datepicker__day__effect"></span>
          <span class="datepicker__day__text">{{ day.format('D') }}</span>
        </div>
      </div>
    </div>
    <div class="datepicker__actions">
      <button @click="cancel">Annuler</button>
      <button @click="submit">Ok</button>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import Month from '../../modules/month.js'

export default {
  props: {
    date: {},
    visible: {type: Boolean, default: true}
  },
  data () {
    let days_lg = []
    for (var id = 0; id < 7; id++) {
      days_lg[id] = moment.weekdaysMin(moment().startOf('week').isoWeekday() + id)
    }
    return {
      days: days_lg,
      month: new Month(this.date.month(), this.date.year()),
      classDirection: 'off',
      dayDirection: 'off'
    }
  },
  methods: {
    isSelected (day) {
      return this.date.unix() === day.unix()
    },
    selectDate (day) {
      this.dayDirection = 'direction-next'
      if (day.isBefore(this.date)) {
        this.dayDirection = 'direction-prev'
      }
      this.date = day.clone()
    },
    nextMonth () {
      this.classDirection = 'direction-next'
      let month = this.month.month + 1
      let year = this.month.year
      if (month > 11) {
        year += 1
        month = 0
      }
      this.month = new Month(month, year)
    },
    prevMonth () {
      this.classDirection = 'direction-prev'
      let month = this.month.month - 1
      let year = this.month.year
      if (month < 0) {
        year -= 1
        month = 11
      }
      this.month = new Month(month, year)
    },
    nextYear () {
      this.classDirection = 'direction-next'
      let month = this.month.month
      let year = this.month.year + 1
      if (year > 3000) {
        year = 3000
      }
      this.month = new Month(month, year)
    },
    prevYear () {
      this.classDirection = 'direction-prev'
      let month = this.month.month
      let year = this.month.year - 1
      if (year < 1970) {
        year = 1970
      }
      this.month = new Month(month, year)
    },
    submit () {
      this.$dispatch('change', this.date)
    },
    cancel () {
      this.$dispatch('cancel')
    }
  },
  watch: {
    visible (val, oldval) {
      if (val === false) {
        this.classDirection = 'off'
        this.dayDirection = 'off'
      }
    }
  },
  computed: {
    year () {
      return this.date.format('YYYY')
    },
    classWeeks () {
      return 'has-' + this.month.getWeeks() + '-weeks'
    },
    date_formatted () {
      return this.date.format('dddd DD MMM')
    }
  }
}
</script>
