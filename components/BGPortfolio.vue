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
          Portfolio - Top ten projects
        </h3>
        <p class="mt-3 text-gray-600">
          
        </p>
        <div class="grid grid-cols-1 gap-y-10 sm:grid-cols-2 gap-x-6 lg:grid-cols-3 xl:grid-cols-4 xl:gap-x-8">
          <div v-for="(project, index) in projects" :key="index" class="group">
            <div class="w-full aspect-w-1 aspect-h-1 bg-gray-200 rounded-lg overflow-hidden xl:aspect-w-7 xl:aspect-h-8">
              <canvas :ref="project.id">
              </canvas>
            </div>
            <h3 class="mt-1 text-lg font-medium text-gray-700 flex justify-between">
              <span>
                <span class="text-gray-900 text-xl">{{project.id}}. </span>
                <span class="border-b border-solid border-gray-900">{{project.name}}</span>
              </span>
              <span class="text-base pt-1 text-gray-500">{{project.year}}</span>
            </h3>
            <p class="mt-1 text-sm text-gray-900">
              {{project.description}}
            </p>
          </div>
        </div>
      </div>
      <BGFooter/>
    </div>
  </div>
</template>

<script>
import { fabric } from "fabric";
export default {
  name: 'BGPortfolio',
  data() {
    return {
      projects: [
        {
          "id": 1,
          "name": "Decision Capital",
          "industry": "FinTech",
          "year": 2019,
          "description": "Web for foreign currency exchange. It was developed from scratch",
          "tech": "PHP",
          "frameworks": "Laravel, VueJS"
        },
        {
          "id": 2,
          "name": "Mamás Agua Sana",
          "industry": "Sales",
          "year": 2021,
          "description": "Progressive Web App for selling water. It was developed from scratch",
          "tech": "PHP",
          "frameworks": "Laravel, VueJS"
        },
        {
          "id": 3,
          "name": "Quipu Mina",
          "industry": "Mining",
          "year": 2015,
          "description": "Mobile application and web platform for time management of mining equipment. SMS as communication channel",
          "tech": "Java",
          "frameworks": "Android, JEE"
        },
        {
          "id": 4,
          "name": "Sampling and Logging For Geologists",
          "industry": "Mining",
          "year": 2015,
          "description": "Web app for sampling and logging using 2D map",
          "tech": "JS",
          "frameworks": "BackboneJS"
        },
        {
          "id": 5,
          "name": "Metro de Lima by BGL",
          "industry": "Transportation",
          "year": 2012,
          "description": "Mobile application for getting the train schedules. Personal projecto. It reached 1.5k active users in 2013",
          "tech": "Java",
          "frameworks": "Android"
        },
        {
          "id": 6,
          "name": "Planning System",
          "industry": "Mining",
          "year": 2015,
          "description": "Web platform to insert planning data",
          "tech": "Java, JS",
          "frameworks": "JEE"
        },
        {
          "id": 7,
          "name": "Fast Data Updater",
          "industry": "Mining",
          "year": 2016,
          "description": "Script for automation of batch updates for third-party software",
          "tech": "AutoHotKey",
          "frameworks": "-"
        },
        {
          "id": 8,
          "name": "Ethical Chatbot",
          "industry": "Beauty",
          "year": 2018,
          "description": "Chatbot for ethical Q&A",
          "tech": "JS",
          "frameworks": "NodeJS, Bot framework"
        },
        {
          "id": 9,
          "name": "AquaMype",
          "industry": "Aquaculture",
          "year": 2020,
          "description": "Water quality monitoring prototype in real time to improve the productivity of rainbow trout in Amazon region.",
          "tech": "Python, JS",
          "frameworks": "Django, React Native"
        },
        {
          "id": 10,
          "name": "Monipez",
          "industry": "Fishing",
          "year": 2019,
          "description": "Monitoring prototype to optimize artisanal capture processes, in real time, in order to improve the productivity of artisanal fishermen in the Chorrillos Cove",
          "tech": "Python, JS",
          "frameworks": "Flask, ReactJS, Android"
        }
      ],
      canvas: {},
      rects: {},
    }
  },
  mounted() {
    this.$nextTick(() => {
        window.addEventListener("resize", this.resizeCanvas);
        this.resizeCanvas();
    })
    this.resizeCanvas()
  },
  methods: {
    resizeCanvas(){
      this.projects.forEach(p => {
        const id = p.id
        const container = this.$refs[id][0].parentElement
        if(container){
          const width = container.parentElement.offsetWidth
          if(!this.canvas[id]){
            this.canvas[id] = new fabric.StaticCanvas(
                this.$refs[id][0],
                { selection: false }
              )
              this.rects[id] = new fabric.Rect({
                top : 0,
                left : 0,
                width,
                height : 20,
                fill : 'red'
            });
            this.canvas[id].add(this.rects[id]);
          }
          this.rects[id].width = width
          this.canvas[id].setWidth(width)
          this.canvas[id].renderAll()
        }
      })
    }
  },
}
</script>
