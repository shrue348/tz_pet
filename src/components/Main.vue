<template>
  <div class="main">
    <div class="form">
      <label class="form_label">Выберите дату</label>
      <datepicker 
        :placeholder="datePicker.placeholder" 
        :language="datePicker.ru" 
        :format="datePickerFormat" 
        :value="datePicker.date" 
        :monday-first="true" 
        :highlighted="datePicker.highlighted"
      ></datepicker>

      <div class="small">Используется библиотека vuejs-datepicker@1.5.4: <a target="_blank" href="https://github.com/charliekassel/vuejs-datepicker">https://github.com/charliekassel/vuejs-datepicker</a></div>
    </div>
  </div>
</template>


<script>
import Datepicker from 'vuejs-datepicker'
import { ru } from 'vuejs-datepicker/dist/locale'

export default {
  name: 'Main',
  components: {
    Datepicker
  },
  data () {
    return {
      datePicker: {
        date: '',
        ru: ru,
        placeholder: 'wef',
        highlighted: {
          dates: [],
          days: []
        }
      },
      test: ''
    }
  },
  methods: {
    datePickerFormat(date){
      let dayNames = ['Воскресенье', 'Понедельник', 'Вторник', 'Среда', 'Четверг', 'Пятница', 'Суббота'],
          day,
          dayNum,
          dayDate,
          lastDayOfMonth;

      if (date) day = new Date(date)
      else day = new Date(this.datePicker.date)

      dayNum = day.getDay()
      dayDate = day.getDate()

      lastDayOfMonth = new Date(new Date(day).getFullYear(), new Date(day).getMonth() + 1, 0);
      lastDayOfMonth = lastDayOfMonth.getDate()

      this.datePicker.highlighted.dates.length = 0

      while ( dayDate <= lastDayOfMonth || dayDate <= 31) {
        console.log(dayDate, lastDayOfMonth)
        this.datePicker.highlighted.dates.push(new Date( new Date(day).getFullYear(), new Date(day).getMonth(), dayDate) )
        dayDate+=7
      }

      return dayNames[dayNum]
    },

  },
  created(){
    this.datePicker.placeholder = this.datePickerFormat(Date.now()) // ставим в плейсолдер сегодняшний день недели
    this.datePicker.highlighted.dates.length = 0 // обнуляем подсветку следующих
  }
}
</script>


<style lang="scss">
body { margin: 0; padding: 0; background-color: #ECEFF1; color: #323432; font-family: 'Avenir', Helvetica, Arial, sans-serif; }
.app { display: flex; align-items: center; height: 100vh; justify-content: center; }

.form { 
  text-align: left; max-width: 280px; margin: 0 auto; 

  & input[type="text"] { border-radius: 2px; -moz-border-radius: 2px; -webkit-border-radius: 2px; padding: .65em .8em; border: 1px solid #bfc1c3; width: 100%; max-width: 100%; box-sizing: border-box; background-color: #fff; -webkit-appearance: none; font-size: 14px; }
  &_label { margin-bottom: .2em; font-size: .8em; display: block; }
}

.small { font-size: .8em; margin-top: 3em; }
a { color: #212121; }

.vdp-datepicker__calendar .cell:not(.blank):not(.disabled).day:hover, 
.vdp-datepicker__calendar .cell:not(.blank):not(.disabled).month:hover, 
.vdp-datepicker__calendar .cell:not(.blank):not(.disabled).year:hover { border: 1px solid #7CB342; }
.vdp-datepicker__calendar .cell.highlighted { background-color: #DCEDC8; }
.vdp-datepicker__calendar .cell.selected.highlighted { background-color: #80DEEA; }
.vdp-datepicker__calendar .cell.today { 
  position: relative;
  &:after { position: absolute; width: .5em; height: .5em; border-radius: .5em; content: ''; background-color: #ff0000; top: .2em; right: .1em; }
}
</style>
