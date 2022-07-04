<!-- Please remove this file from your project -->
<template>
  <div
    class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0"
  >
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <div class="mt-8 bg-white overflow-hidden shadow sm:rounded-lg p-6 pb-1">
        <h2 class="text-2xl leading-7 font-bold">
          Billy Grados
        </h2>
        <h3 class="text-lg leading-7 font-semibold">
          Skills - 
          <select v-model="display" @change="toggle" class="text-sm">
            <option value="1">Top advanced ones</option>
            <option value="0">More</option>
          </select>
        </h3>
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
    "display": 1,
    "yearsC": 11,
    "yearsS": 9,
    "skill": "JavaScript"
  },
  {
    "category": 1,
    "display": 1,
    "yearsC": 7,
    "yearsS": 4,
    "skill": "PHP"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "VueJS"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 3,
    "yearsS": 2,
    "skill": "ReactJS"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 3,
    "yearsS": 2,
    "skill": "React Native"
  },
  {
    "category": 1,
    "display": 1,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "Python"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "Node.js"
  },
  {
    "category": 1,
    "display": 1,
    "yearsC": 6,
    "yearsS": 5,
    "skill": "Java"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 6,
    "yearsS": 5,
    "skill": "Android"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 9,
    "yearsS": 8,
    "skill": "HTML5"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 10,
    "yearsS": 9,
    "skill": "Git"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 1,
    "yearsS": 1,
    "skill": "AngularJS"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 10,
    "yearsS": 8,
    "skill": "Software Development"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 9,
    "yearsS": 7,
    "skill": "Web Development"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 9,
    "yearsS": 7,
    "skill": "Full Stack Development"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 2,
    "yearsS": 1,
    "skill": "PWA"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 7,
    "yearsS": 6,
    "skill": "Mobile Development"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 12,
    "yearsS": 10,
    "skill": "Research & Development"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 10,
    "yearsS": 9,
    "skill": "Innovation in Technology"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 10,
    "yearsS": 9,
    "skill": "IT Management"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 10,
    "yearsS": 9,
    "skill": "Project Management"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 6,
    "yearsS": 5,
    "skill": "Cloud Computing"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 1,
    "yearsS": 0,
    "skill": "Wordpress Development"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 2,
    "yearsS": 1,
    "skill": "Google Apps Script"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 8,
    "yearsS": 7,
    "skill": "Scrum"
  },
  {
    "category": 3,
    "display": 1,
    "yearsC": 3,
    "yearsS": 3,
    "skill": "MS-SQL"
  },
  {
    "category": 3,
    "display": 1,
    "yearsC": 6,
    "yearsS": 4,
    "skill": "MySQL"
  },
  {
    "category": 3,
    "display": 1,
    "yearsC": 1,
    "yearsS": 1,
    "skill": "MongoDB"
  },
  {
    "category": 3,
    "display": 1,
    "yearsC": 2,
    "yearsS": 2,
    "skill": "PostgreSQL"
  },
  {
    "category": 3,
    "display": 0,
    "yearsC": 1,
    "yearsS": 0,
    "skill": "Oracle Database"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 4,
    "yearsS": 3,
    "skill": "Laravel"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 3,
    "yearsS": 2,
    "skill": "Inertia"
  },
  {
    "category": 2,
    "display": 1,
    "yearsC": 1,
    "yearsS": 1,
    "skill": "Django"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 2,
    "yearsS": 2,
    "skill": "SharePoint Online"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 3,
    "yearsS": 2,
    "skill": "MQTT"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 2,
    "yearsS": 1,
    "skill": "LaTeX"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 2,
    "yearsS": 2,
    "skill": "AutoHotKey"
  },
  {
    "category": 5,
    "display": 0,
    "yearsC": 12,
    "yearsS": 10,
    "skill": "Creativity"
  },
  {
    "category": 5,
    "display": 0,
    "yearsC": 12,
    "yearsS": 10,
    "skill": "Flexibility"
  },
  {
    "category": 5,
    "display": 0,
    "yearsC": 9,
    "yearsS": 8,
    "skill": "Communication"
  },
  {
    "category": 5,
    "display": 0,
    "yearsC": 12,
    "yearsS": 10,
    "skill": "Analytical skills"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 6,
    "yearsS": 5,
    "skill": "Microsoft Office"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 1,
    "yearsS": 0,
    "skill": "Kotlin"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 0,
    "yearsS": 0,
    "skill": "Ruby"
  },
  {
    "category": 4,
    "display": 0,
    "yearsC": 1,
    "yearsS": 0,
    "skill": "Flask"
  }
]
const CATEGORIES = [
  'Programming Languages', 'Frameworks & tools', 'Databases', 'Others', 
  'Soft skills'
]
const LABEL_REPLACE = {
  'Aplicaciones web progresivas (PWA)': 'PWA',
  'Gestión de Tecnologías de la Información': 'Gestión de TI',
  'Desarrollo de aplicaciones móviles': 'Desarrollo mobile apps',
}
export default {
    name: "BGSkills",
    components: { Bar },
    data() {
      const skillsData = this.getData(1)
      return {
        width: window.innerWidth,
        skillsData,
        display: 1,
        chartData: {
          labels: skillsData.labels,
          datasets: [{ 
            label: 'years',
            data: skillsData.data,
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
            x: {
              max: skillsData.maxYears,
            },
            y: {
              ticks: {
                autoSkip: false,
                callback: (value, index, ticks) => {
                  const label = this.skillsData.labels[index]
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
                return ctx.dataset.data[ctx.dataIndex] === 
                  this.skillsData.maxYears ? 'center' : 'end'
              },
              align: (ctx) => {
                const value = ctx.dataset.data[ctx.dataIndex]
                if(value === this.skillsData.maxYears){
                  return 'center'
                }else if(value >= this.skillsData.maxYears - 1){
                  return 'left'
                }
                return 'right'
              },
              font: (ctx) => {
                return {
                  weight: ctx.dataset.data[ctx.dataIndex] === 
                    this.skillsData.maxYears ? 'bold' : 'normal'
                }
              },
              formatter: (value, context) => {
                if(value === this.skillsData.maxYears){
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
          width: this.width,
          height: this.skillsData.labels.length * 20 + 'px',
          position: 'relative'
        }
      }
    },
    mounted() {
      this.$nextTick(() => {
          window.addEventListener("resize", this.resizeCanvas);
      });
      this.resizeCanvas();
    },
    methods: {
      resizeCanvas(){
        this.width = window.innerWidth < 640 ? '90vw' : '50vw'
      },
      toggle(){
        this.skillsData = this.getData(this.display)
        this.updateChart()
      },
      getData(display){
        const prop = 'yearsS'
        const result = {
          labels: [],
          data: [],
          maxYears: 0
        }          
        let skills = _.chain(SKILLS)
          .sortBy( s => {
            return -s[prop]
          })
          .map( s => {
            s.years = s[prop] < 1 ? 1 : s[prop]
            if(LABEL_REPLACE[s.skill]){
              s.skill = LABEL_REPLACE[s.skill]
            }
            return s
          })
          .filter(['display', parseInt(display)])
          .value()
        result.maxYears = _.maxBy(skills, 'years').years + 1
        skills = _.groupBy(skills, 'category')        
        const labels = []
        const data = []
        _.each(skills, (g, i) => {
          labels.push(CATEGORIES[i - 1])
          labels.push(_.map(g, 'skill'))
          data.push(result.maxYears)
          data.push(_.map(g, 'years'))
        })
        result.labels = _.flatten(labels)
        result.data = _.flatten(data)
        return result
      },
      updateChart(){
        this.chartOptions.scales.x.max = this.skillsData.maxYears
        this.chartData.datasets[0].data = this.skillsData.data
        this.chartData.labels = this.skillsData.labels
      },
    }
}
</script>
