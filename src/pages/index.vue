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
        <div class="i-iconoir-birthday-cake mr-2 inline-block text-4xl -mb-2" />土制活了几天计算器
      </h1>
    </section>
    <div class="mt-8 flex items-center">
      <!-- 好吧这个组件还挺好用的 -->
      <a-date-picker v-model="userBirthday" class="" size="large" :allow-clear="false" />
      <a-button type="primary" class="btn-operate ml-4" @click="showResult">
        <template #icon>
          <div i-mdi-calculator-variant-outline icon-btn />
        </template>
        <template #default>
          <span>帮我算算</span>
        </template>
      </a-button>
    </div>
    <div v-if="result" class="mt-8">
      <div v-if="!result.isAfter" class="common">
        <div>
          已经来到这个世界{{ result.livingDays }}天了呢
        </div>
        <div>
          这<span v-if="result.livingYears">{{ result.livingYears }}年</span><span v-if="result.livingDayRemainder"> {{
            result.livingDayRemainder }}天</span><span v-if="result.livingDayRemainder">（{{ result.yearLivingFra }}年）</span>辛苦啦
        </div>
        <div>
          距离下个破蛋日还有{{ result.dayToNextBirthday }}天哦
        </div>
      </div>
      <div class="extra">
        <div v-if="result.isBirthday && result.livingYears > 0">
          祝你破蛋日过得开心🎂
        </div>
        <div v-if="result.isAfter">
          我超未来人，可以私信告诉我下期双色球编号吗o3o
        </div>
      </div>
    </div>
    <div v-else class="mt-8">
      <div>↑或许这里可以填进破蛋日↑</div>
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
