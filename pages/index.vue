<template>
  <section class="section container content">
    <h1>
      Nuxt.js Advent Calendar 2019
    </h1>

    <p>{{ comment }}</p>

    <b-table
      class="table is-bordered"
      :data="dateIndex"
      :columns="columns"
    >
      <template slot-scope="props">
        <b-table-column
          v-for="index in [0, 1, 2, 3, 4, 5, 6]"
          :key="index"
          :field="columns[index].field"
          :label="props.row.id"
          class="week"
        >

          <p
            v-if="days[props.row[dateKeys[index]]]"
            class="tag is-primary is-medium"
          >
            {{ props.row[dateKeys[index]] }}
          </p>

          <p v-else class="tag is-light is-medium">
            {{ props.row[dateKeys[index]] }}
          </p>

          <article
            v-if="days[props.row[dateKeys[index]]]"
            class="media"
          >
            <div class="media-content">
              <img
                class="image is-48x48"
                :src="days[props.row[dateKeys[index]]].authorImageUrl"
                :alt="days[props.row[dateKeys[index]]].authorName"
              >

              <a
                :href="`https://qiita.com/${days[props.row[dateKeys[index]]].authorName}`"
                target="_blank"
                rel="noopener noreferrer"
                class="has-text-weight-normal is-size-6"
              >
                {{ days[props.row[dateKeys[index]]].authorName }}
              </a>

              <div class="media content">
                <a v-if="days[props.row[dateKeys[index]]].articleUrl !== null"
                  :href="days[props.row[dateKeys[index]]].articleUrl"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="has-text-primary is-6"
                >
                  {{ days[props.row[dateKeys[index]]].comment }}
                </a>

                <p v-else
                  class="has-text-dark is-6"
                >
                  {{ days[props.row[dateKeys[index]]].comment }}
                </p>
              </div>
            </div>
          </article>
        </b-table-column>
      </template>
    </b-table>

    <p>※ このページはレスポンシブ対応していません。デスクトップサイズの画面でご覧ください。</p>
  </section>
</template>

<script>
import { parse } from 'node-html-parser'
export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get('https://qiita.com/advent-calendar/2019/nuxt-js')
    const root = parse(data)

    const dayNames = root.querySelectorAll('.adventCalendarCalendar_dayName').map(dayName => dayName.text)
    const columns = [
      { field: 'sun', label: dayNames[0] },
      { field: 'mon', label: dayNames[1] },
      { field: 'tue', label: dayNames[2] },
      { field: 'wed', label: dayNames[3] },
      { field: 'thu', label: dayNames[4] },
      { field: 'fri', label: dayNames[5] },
      { field: 'sat', label: dayNames[6] }
    ]

    return {
      comment: root.querySelector('.markdownContent').text,
      columns: columns,
      days: root.querySelectorAll('.adventCalendarCalendar_day').map(day => {
        const articleUrl = day.querySelector('.adventCalendarCalendar_comment').querySelector('a') ?
          day.querySelector('.adventCalendarCalendar_comment').querySelector('a').attributes['href'] :
          null
        return {
          date: day.querySelector('.adventCalendarCalendar_date').text,
          authorName: day.querySelector('.adventCalendarCalendar_author').text.replace(/\s|&nbsp;/g, ''),
          authorImageUrl: day.querySelector('.adventCalendarCalendar_authorIcon').attributes['src'],
          comment: day.querySelector('.adventCalendarCalendar_comment').text,
          articleUrl: articleUrl
        }
      })
    }
  },
  data() {
    return {
      dateIndex: [0, 1, 2, 3].map(weekNumber => {
        return {
          sun: 0 + (7 * weekNumber),
          mon: 1 + (7 * weekNumber),
          tue: 2 + (7 * weekNumber),
          wed: 3 + (7 * weekNumber),
          thu: 4 + (7 * weekNumber),
          fri: 5 + (7 * weekNumber),
          sat: 6 + (7 * weekNumber)
        }
      }),
      dateKeys: ['sun', 'mon', 'tue', 'wed', 'thu', 'fri', 'sat']
    }
  }
}
</script>

<style>
.week {
  width: calc(100% / 7);
}
</style>
