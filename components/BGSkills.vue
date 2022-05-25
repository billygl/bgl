<!-- Please remove this file from your project -->
<template>
  <div
    class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0"
  >
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <div class="mt-8 bg-white overflow-hidden shadow sm:rounded-lg p-6">
        <h2 class="text-2xl leading-7 font-bold">
          Billy Grados
        </h2>
        <h3 class="text-lg leading-7 font-semibold">
          Skills
        </h3>
        <p class="mt-3 text-gray-600">
          
        </p>
        <div class="w-full">
          <Bar
            :chart-options="chartOptions"
            :chart-data="chartData"
            :styles="myStyles"
          />
        </div>
      </div>
      <BGMenu/>
      <BGFooter/>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
import { Bar } from 'vue-chartjs/legacy'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
import ChartDataLabels from 'chartjs-plugin-datalabels'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, ChartDataLabels)

const SKILLS = [
  {
    "category": 1,
    "yearsC": 11,
    "yearsS": 9,
    "skill": "JavaScript"
  },
  {
    "category": 1,
    "yearsC": 7,
    "yearsS": 4,
    "skill": "PHP"
  },
  {
    "category": 2,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "VueJS"
  },
  {
    "category": 2,
    "yearsC": 1,
    "yearsS": 0,
    "skill": "ReactJS"
  },
  {
    "category": 2,
    "yearsC": 3,
    "yearsS": 2,
    "skill": "React Native"
  },
  {
    "category": 1,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "Python"
  },
  {
    "category": 2,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "Node.js"
  },
  {
    "category": 1,
    "yearsC": 6,
    "yearsS": 5,
    "skill": "Java"
  },
  {
    "category": 2,
    "yearsC": 6,
    "yearsS": 5,
    "skill": "Android"
  },
  {
    "category": 2,
    "yearsC": 10,
    "yearsS": 9,
    "skill": "Git"
  },
  {
    "category": 3,
    "yearsC": 3,
    "yearsS": 3,
    "skill": "MS-SQL"
  },
  {
    "category": 3,
    "yearsC": 6,
    "yearsS": 4,
    "skill": "MySQL"
  },
  {
    "category": 3,
    "yearsC": 1,
    "yearsS": 1,
    "skill": "MongoDB"
  },
  {
    "category": 3,
    "yearsC": 2,
    "yearsS": 2,
    "skill": "PostgreSQL"
  },
  {
    "category": 3,
    "yearsC": 1,
    "yearsS": 0,
    "skill": "Oracle Database"
  },
  {
    "category": 2,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "Laravel"
  },
  {
    "category": 2,
    "yearsC": 1,
    "yearsS": 1,
    "skill": "Django"
  }
]
const CATEGORIES = [
  'Programming Languages', 'Frameworks & tools', 'Databases'
]
const MAX_YEARS = 10
let labels = []
export default {
    name: "BGSkills",
    components: { Bar },
    data() {
      const skills = _.chain(SKILLS)
      .sortBy( s => {
        return -s.yearsS
      })
      .map( s => {
        s.years = s.yearsS < 1 ? 1 : s.yearsS
        return s
      })
      .groupBy('category')
      .value()
      let data = []
      _.each(skills, (g, i) => {
        labels.push(CATEGORIES[i - 1])
        labels.push(_.map(g, 'skill'))
        data.push(MAX_YEARS)
        data.push(_.map(g, 'years'))
      })
      labels = _.flatten(labels)
      data = _.flatten(data)
      return {
        chartData: {
          labels,
          datasets: [{ 
            label: 'years',
            data,
            borderWidth: 2,
            borderRadius: 20,
          }],
        },
        chartOptions: {
          indexAxis: 'y',
          elements: {
            bar: {
              borderWidth: 2,
            }
          },
          scales: {
            y: {
              ticks: {
                  autoSkip: false,
                  callback: (value, index, ticks) => {
                    const label = labels[index]
                    return CATEGORIES.includes(label) ? '' : label
                  }
              }
            }
          },
          plugins: {
            legend: {
              position: 'bottom',
              onClick: null
            },
            tooltip: {
              enabled: false
            },
            datalabels: {
              anchor: (ctx) => {
                return ctx.dataset.data[ctx.dataIndex] === MAX_YEARS ?
                  'center' : 'end'
              },
              align: (ctx) => {
                return ctx.dataset.data[ctx.dataIndex] === MAX_YEARS ?
                  'center' : 'right'
              },
              font: (ctx) => {
                return {
                  weight: ctx.dataset.data[ctx.dataIndex] === MAX_YEARS ?
                    'bold' : 'normal'
                }
              },
              formatter: (value, context) => {
                if(value === MAX_YEARS){
                  return context.chart.data.labels[context.dataIndex]
                }
                return value;
              }
            }
          },
          responsive: true,
          maintainAspectRatio: false,
        }
      }
    },
    computed: {
      myStyles () {
        return {
          // width: '90vw',
          // height: '65vh',
          position: 'relative'
        }
      }
    }
}
</script>
