<script setup lang="ts">
import dayjs from 'dayjs/esm'

defineOptions({
  name: 'IndexPage',
})

const today = dayjs().startOf('day')
const defaultDate = dayjs(`2000-${today.format('MM-DD')}`).toDate()
const userBirthday = ref(defaultDate)

function logic(input: dayjs.Dayjs) {
  const isBirthday = input.get('month') === today.get('month') && input.get('date') === today.get('date')
  const isAfter = input.isAfter(today) && !input.isSame(today)
  const currentYear = today.get('year')
  const currentYearBirthday = input.clone().set('year', currentYear)

  const isAfterCurrentYearBirthday = !today.isBefore(currentYearBirthday)
  const nextYearBirthday = currentYearBirthday.clone().add(1, 'year')
  const lastYearBirthday = currentYearBirthday.clone().subtract(1, 'year')
  const nextBirthday = isAfterCurrentYearBirthday ? nextYearBirthday : currentYearBirthday
  const lastBirthday = isAfterCurrentYearBirthday ? currentYearBirthday : lastYearBirthday
  const dayToNextBirthday = nextBirthday.diff(today, 'day')
  const dayAfterLastBirthday = today.diff(lastBirthday, 'day')
  const livingYears = today.diff(input, 'year')
  const livingDays = today.diff(input, 'day') + 1
  const livingDayRemainder = dayAfterLastBirthday + 1

  const yearLivingFra = (livingYears + (livingDayRemainder / 365)).toFixed(2)

  return { isAfter, isBirthday, dayToNextBirthday, dayAfterLastBirthday, livingYears, livingDays, livingDayRemainder, yearLivingFra }
}

const result = ref(null)

function showResult() {
  const input = dayjs(userBirthday.value)
  result.value = logic(input)
}
</script>

<template>
  <div>
    <section class="title">
      <h1 text-2xl op90>
        <div class="i-iconoir-birthday-cake mr-2 inline-block text-4xl -mb-2" />活了几天计算器<span ml-2 text-base op50>土制版</span>
      </h1>
    </section>
    <div class="mt-8 flex items-center">
      <!-- 好吧这个组件还挺好用的 -->
      <a-date-picker v-model="userBirthday" class="picker mr-4" size="large" :allow-clear="false" />
      <a-button type="primary" class="btn-operate" @click="showResult">
        <template #icon>
          <div i-mdi-calculator-variant-outline icon-btn />
        </template>
        <template #default>
          <span>帮我算算</span>
        </template>
      </a-button>
    </div>
    <div v-if="result" class="mt-8 text-base">
      <div v-if="!result.isAfter" class="common">
        <div>
          已经来到这个世界{{ result.livingDays }}天了
        </div>
        <div>
          也可以算作【<span><span v-if="result.livingYears">{{ result.livingYears }}年</span><span
            v-if="result.livingDayRemainder"
          > {{
            result.livingDayRemainder }}天</span>】<span v-if="result.livingDayRemainder">或者【{{ result.yearLivingFra
          }}年】</span>
          </span>
        </div>
        <div>
          距离下次生日还有{{ result.dayToNextBirthday }}天
        </div>
      </div>
      <div class="extra">
        <div v-if="result.isBirthday && result.livingYears > 0">
          🎂祝你今天生日快乐~
        </div>
        <div v-if="result.isAfter">
          我超未来人，可以告诉我下期双色球编号吗o3o
        </div>
      </div>
    </div>
    <div v-else class="mt-8 text-base">
      <div>或许↑这里↑可以填入生日</div>
    </div>

    <div py-8 />
  </div>
</template>

<style scoped lang="scss">
:deep(.btn-operate) {
  .arco-btn-icon {
    display: flex;
  }
}
</style>
