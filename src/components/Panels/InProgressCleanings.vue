<template>
    <div class="card">
        <div class="card-header bg-info">
            <div class="row">
                <div class="col-lg-6 col-xs-12">
                    <h5>Limpiezas en progreso</h5>
                </div>
                <div class="col-lg-6 col-xs-12 d-flex justify-content-end">
                    <div class="alert alert-success resume-completed" role="alert">
                        <strong>Terminadas: 0</strong>
                    </div>
                    <div class="alert alert-danger resume-completed" role="alert">
                        <strong>Pendientes: 0</strong>
                    </div>
                    <div class="progress-circle resume-completed d-flex">
                        <div class="navigation_pages">
                            <span @click="navigatePrev"> &#60; </span>
                            <span @click="navigateNext"> &#62; </span>
                        </div>
                        <p>{{page}}/{{numberPagination}}</p>
                    </div>
                </div>
            </div>
        </div>
        <div class="card-body p-3">
            <div class="row justify-content-center">
                <div class="col-lg-4 col-md-6 col-xs-12 item-work">
                    Hora de la Computadora {{hours}} - En segundos {{hoursInSeg}}
                    <br>
                    <div class="item" v-for="(item, index) in listDataxPage" :key="index">
                        <div class="media">
                            <div class="d-flex justify-content-center content-image">
                                <img src="http://pluspng.com/img-png/download-happy-person-png-images-transparent-gallery-advertisement-275.png"
                                     width="75" height="75"/>
                            </div>
                            <div class="media-body">
                                <h5 class="mt-0">{{item.owner_department}}</h5>
                                <p>Limpieza : <span>{{item.type_cleaning}}</span></p>
                                <p>Depa : <span>{{item.user_staff}}</span></p>
                            </div>
                        </div>
                        <div>
                            Tiempo de limpieza: {{item.time}}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import jsonDataClean from '../../data/jsondata.json'

    export default {
        name: "inProgressPanel",
        components: {

        },
        data() {
            return {
                dataCleanDepartment : jsonDataClean,
                numberDataxPage : 6,
                page : 1,
                time : 0,
                hours : '',
                hoursInSeg : ''
            }
        },
        async mounted() {
            setInterval(() => {
                let time = new Date();
                this.hours = time.getHours() + ':' + time.getMinutes() + ':' + time.getSeconds();
                this.hoursInSeg = time.getHours()*3600 + time.getMinutes()*60 + time.getSeconds();
                }, 1000);
                
        },
        computed: {
            numberPagination () {    
                 console.log(jsonDataClean.length, 'cantidad de data', jsonDataClean.length % this.numberDataxPage);             
                if(jsonDataClean.length % this.numberDataxPage !==0){
                    return  Math.floor(jsonDataClean.length / this.numberDataxPage)+1
                }else {
                    return  Math.floor(jsonDataClean.length / this.numberDataxPage)
                }                
               
            },
            listDataxPage(){
                // eslint-disable-next-line vue/no-async-in-computed-properties
                setTimeout(() => {
                    this.navigateNext();
                }, 5000); 
            
                let dataListar = this.dataCleanDepartment.filter ((element, index)=>{                
                                                  
                if ((this.page-1)* this.numberDataxPage <= index && this.page * this.numberDataxPage > index) {
                    
                        const partHorasObject = element.time.split(':');
                        console.log('array Separdo', partHorasObject);
                        const hoursInSegundos = parseInt(partHorasObject[0].trim())*3600;
                        const minutosInSegundos = parseInt(partHorasObject[1].trim())*60;
                        const segundos = parseInt(partHorasObject[2].trim())
                        const horaObjectInSegunds = hoursInSegundos + minutosInSegundos + segundos;
                        console.log('TOTAL ', horaObjectInSegunds  );
                        
                        console.log('Hora en Segundos', hoursInSegundos, 'MINUTOS', minutosInSegundos, 'SEGUNDOS', segundos);
                        
                        
                       console.log('ELEMENTO' , element.time);        
                        return this.dataCleanDepartment[index];                            
                    }
                })
                
                return dataListar;
            }


        },
        methods: {
            loadStatus() {
                this.$store.dispatch('home/accessStatusCleaning', this.$parent.auth)
            },
            loadCleanings() {
                this.$store.dispatch('home/progressCleaning', this.$parent.auth);
            },
            navigatePrev() {
               this.page == 1 ? this.page = 1 : this.page -=1;

            },
            navigateNext() {
              this.numberPagination == this.page ? this.page = 1 : this.page +=1;
            },
            convertirHoraASegundos (){
            
            },

            convertirSegundosAHora(){

            }

        
            
        }
    }
</script>

<style scoped lang="scss">

</style>
