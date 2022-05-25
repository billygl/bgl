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
        <div v-show="isLoading" class="grid grid-cols-1">
          <div class="flex justify-between text-gray-600">
            Loading... <ImgSpinner/>
          </div>
          <div class="w-full aspect-w-1 aspect-h-1 xl:aspect-w-7 xl:aspect-h-8 overflow-hidden bg-gray-100 rounded-lg">
            <canvas ref="loading" class="w-full h-full object-center object-cover">
            </canvas>
          </div>
        </div>
        <div 
          v-show="isSetup"
          class="grid grid-cols-1 gap-y-10 sm:grid-cols-2 gap-x-6 lg:grid-cols-3 xl:grid-cols-4 xl:gap-x-8">
          <div v-for="(project, index) in projects" :key="index" class="group">
            <div class="w-full aspect-w-1 aspect-h-1 bg-gray-200 rounded-lg overflow-hidden xl:aspect-w-7 xl:aspect-h-8">
              <canvas :ref="project.id" class="w-full h-full object-center object-cover group-hover:opacity-75">
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
      <BGMenu/>
      <BGFooter/>
    </div>
  </div>
</template>

<script>
import { fabric } from "fabric";
import _ from "lodash";
const ARRAY_SEPARATOR = ','
const PROJECTS = [
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
    "frameworks": ""
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
]
const IMAGE_PROJECTS = "images/projects/"
const IMAGE_TECHS = "images/techs/"
const IMAGE_EXTENSION_SVG = ".svg"
const IMAGE_EXTENSION_JPG = ".jpg"
export default {
    name: "BGPortfolio",
    data() {
        const projects = PROJECTS.map(p => {
            const q = {
              ...p,
              industry: p.industry.toLowerCase(),
              tech: this.slug(p.tech),
              frameworks: this.slug(p.frameworks),
            }
            q.tech = q.tech ? q.tech.split(ARRAY_SEPARATOR) : [];
            q.frameworks = q.frameworks ? q.frameworks.split(ARRAY_SEPARATOR) : [];
            return q;
        });
        return {
            projects,
            canvas: {},
            imgs: {},
            frameworks: [],
            currentFramework: 0,
            loadingCanvas: null,
            isSetup: false,
            isLoading: true,
        };
    },
    mounted() {
        this.setupLoading()
        this.$nextTick(() => {
            window.addEventListener("resize", this.resizeCanvas);
        });
        this.resizeCanvas();
    },
    methods: {
        drawImage(image, canvas, containerWidth, containerHeight){
          const ratio = containerWidth / containerHeight;
          const corner = {
              left: 0,
              top: 0,
          };
          const breakRatio = 1.2;
          if (ratio > breakRatio) {
              image.scaleToHeight(containerHeight);
          }
          else if (ratio <= breakRatio) {
              image.scaleToWidth(containerWidth);
          }
          const height = image.height * image.scaleY;
          const width = image.width * image.scaleX;
          if (ratio > breakRatio) {
              corner.left = (containerWidth - width) / 2;
          }
          else if (ratio <= breakRatio) {
              corner.top = (canvas.height - height) / 2;
          }
          image.set(corner);
          return {
            width,
            height,
            corner
          }
        },
        drawTech(){
          const path = IMAGE_TECHS + 
            this.frameworks[this.currentFramework] + IMAGE_EXTENSION_SVG;
          const element = this.$refs.loading
          fabric.Image.fromURL(path, (oImg) => {
            this.loadingCanvas.clear()
            this.loadingCanvas.add(oImg)
            const containerHeight = element.parentElement.offsetHeight
            const containerWidth = element.parentElement.offsetWidth
            this.loadingCanvas.setWidth(containerWidth)
            this.drawImage(oImg, this.loadingCanvas, containerWidth, containerHeight)
            this.loadingCanvas.renderAll()
          })
        },
        updateDrawTech(){
          if(this.isLoading){
            this.drawTech()
            setTimeout( () => {
              this.currentFramework = 
                  this.currentFramework === this.frameworks.length - 1 ? 
                  0 : this.currentFramework + 1
              this.updateDrawTech()
            }, 800)
          }
        },
        setupLoading(){
          const frameworks = _.chain(this.projects)
            .map('frameworks')
            .flatten()
            .uniq()
            .value()
          const tech = _.chain(this.projects)
            .map('tech')
            .flatten()
            .uniq()
            .value()
          this.frameworks = _.concat(tech, frameworks)
          this.loadingCanvas = new fabric.StaticCanvas(this.$refs.loading)
          this.updateDrawTech()
          setTimeout(() => {
            this.drawTech()
          }, 1) 
        },
        slug(str) {
            return str.toLowerCase().replaceAll(" ", "");
        },
        addImage(type, id, path, canvas) {
            return new Promise((resolve) => {
                fabric.Image.fromURL(path, (oImg) => {
                    const imgs = this.imgs[id] || {};
                    if (type === "background") {
                        canvas.sendToBack(oImg);
                        imgs[type] = oImg;
                    }
                    else {
                        const rect = new fabric.Rect({
                            width: oImg.width,
                            height: oImg.height,
                            fill: "#fff",
                            opacity: 0.65
                        });
                        const group = new fabric.Group([rect, oImg]);
                        canvas.add(group);
                        imgs[type] = imgs[type] || [];
                        imgs[type].push(group);
                    }
                    this.imgs[id] = imgs;
                    resolve();
                });
            });
        },
        setup(p, canvas, container) {
            const id = p.id;
            const promises = [];
            // background
            let image;
            image = IMAGE_PROJECTS + p.industry + IMAGE_EXTENSION_JPG;
            promises.push(this.addImage("background", id, image, canvas));
            // p.tech & p.frameworks
            p.tech.forEach(t => {
                image = IMAGE_TECHS + t + IMAGE_EXTENSION_SVG;
                promises.push(this.addImage("tech", id, image, canvas));
            });
            p.frameworks.forEach(f => {
                image = IMAGE_TECHS + f + IMAGE_EXTENSION_SVG;
                promises.push(this.addImage("frameworks", id, image, canvas));
            });
            Promise.all(promises)
                .then(() => {
                this.isSetup = true
                setTimeout(() => {
                  this.updateCanvas(id, container)
                }, 20)
                setTimeout(() => {
                  this.isLoading = false
                }, 2000)
            });
            return promises;
        },
        updateCanvas(id, container) {
            const group = this.imgs[id];
            if (group) {
                const containerWidth = container.offsetWidth;
                const containerHeight = container.offsetHeight;
                // background
                const background = group.background;
                const dimen = this.drawImage(
                  background, this.canvas[id], containerWidth, containerHeight
                )
                // tech & frameworks
                const techHeight = dimen.height / 3;
                group.tech.forEach((t, index) => {
                    t.set({
                        left: dimen.corner.left,
                        top: dimen.corner.top + index * techHeight + 5
                    });
                    t.scaleToHeight(techHeight);
                });
                const frameworkHeight = dimen.height * 0.22;
                group.frameworks && group.frameworks.forEach((f, index) => {
                    f.scaleToHeight(frameworkHeight);
                    const fWidth = f.width * f.scaleX;
                    f.set({
                        left: dimen.corner.left + dimen.width - fWidth - 10,
                        top: dimen.corner.top + index * frameworkHeight + 5,
                    });
                });
                this.canvas[id].setWidth(containerWidth);
                this.canvas[id].renderAll();
            }
        },
        resizeCanvas() {
            this.drawTech()
            this.projects.forEach(p => {
                const id = p.id;
                const container = this.$refs[id][0].parentElement;
                if (container) {
                    if (!this.canvas[id]) {
                        this.canvas[id] = new fabric.StaticCanvas(this.$refs[id][0]);
                        this.setup(p, this.canvas[id], container);
                    }
                    else {
                        this.updateCanvas(id, container)
                    }
                }
            });
        }
    }
}
</script>
