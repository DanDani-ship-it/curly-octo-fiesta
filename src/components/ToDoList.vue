<script>
export default{
    data(){
        return{
            tareasPendientes: [],
            nuevaTarea: "",
            tareasCompletadas: []
        };
    },
    methods:{
        agregarTarea(){
            
            if(this.nuevaTarea.trim()){
                this.tareasPendientes.push({ texto: this.nuevaTarea});
                this.nuevaTarea = "";
                this.guardarTareas();
            }
        },
        eliminarTareas(index, lista){
            if (lista === "pendientes") {
                this.tareasPendientes.splice(index, 1);
            } else if (lista === "completadas") {
                this.tareasCompletadas.splice(index, 1);
            }
            this.guardarTareas();
        },
        guardarTareas(){
            localStorage.setItem("tareasPendientes", JSON.stringify(this.tareasPendientes));
            localStorage.setItem("tareasCompletadas", JSON.stringify(this.tareasCompletadas));
            
        },
        cargarTareas(){
            const pendientes = localStorage.getItem("tareasPendientes");
            const completadas = localStorage.getItem("tareasCompletadas");
            if (pendientes) this.tareasPendientes = JSON.parse(pendientes);
            if (completadas) this.tareasCompletadas = JSON.parse(completadas);
        },
        completarTareas(index){
            const tarea = this.tareasPendientes.splice(index, 1)[0]; 
            this.tareasCompletadas.push(tarea); 
            this.guardarTareas(); 
        }
    },
    mounted(){
        this.cargarTareas();
    }
}
</script>

<template>
   
        <h2 class="text-center mt-5 ">Mis Tareas</h2>
        <div class="container">
            <div class="input-group mb-3 ">
                <input type="text" class="form-control"  v-model="nuevaTarea" placeholder="Agregar Tarea" aria-label="Recipient's username" aria-describedby="button-addon2">
                <button class="btn btn-outline-secondary" type="button" id="button-addon2" @click="agregarTarea">+</button>
            </div>

            <h4 class="text-center">Tareas Por Hacer</h4>
            <!-- <ol class="list-group list-group-numbered lista-group">
                <li class="list-group-item d-flex justify-content-between align-items-start lista" v-for="(tarea, index) in tareasPendientes"
                :key="'pendiente-' + index">
                    <div class="ms-2 me-auto">
                    <div >{{ tarea.texto }}</div>
                    </div>
                    <button
                        class="btn btn-sm btn-danger btn-v"
                        @click="completarTareas(index)"
                    >
                    ✓
                    </button>
                </li>
            </ol> -->

            <transition-group name="listas" tag="ol" class="list-group list-group-numbered lista-group">
                <li class="list-group-item d-flex justify-content-between align-items-start lista"
                    v-for="(tarea, index) in tareasPendientes" :key="'pendiente-' + index">
                    <div class="ms-2 me-auto">
                        <div>{{ tarea.texto }}</div>
                    </div>
                    <button class="btn btn-sm btn-success" @click="completarTareas(index)">✓</button>
                </li>
            </transition-group>
            <h4 class="text-center mt-3">Tareas Hechas</h4>
            <transition-group name="listas" tag="ol" class="list-group list-group-numbered lista-group">
                <li class="list-group-item d-flex justify-content-between align-items-start lista" v-for="(tarea, index) in tareasCompletadas"
                :key="'completada-' + index">
                    <div class="ms-2 me-auto">
                    <div >{{ tarea.texto }}</div>
                    </div>
                    <button
                        class="btn btn-sm btn-danger btn-x"
                        @click="eliminarTareas(index, 'completadas')"
                    >
                    X
                    </button>
                </li>
            </transition-group>
            
        </div>
    

</template>

<style>
    body{
        font-family: "Open Sans", sans-serif !important;
    }
    h2{
        color: rgb(255, 255, 255);
        margin-bottom: 20px;
    }
    h4{
        color: gray;
    }
    .btn-v{
        color: rgb(16, 204, 16) !important;
    }
    .btn-x{
        color: rgb(255, 66, 66) !important;
    }
    .lista{
        background-color: dimgray;
        border-color: dimgray;
        color: white;
    }

   input{
    background-color: dimgrey !important;
    border-color: dimgray !important;
    color: white !important;
   }
   input::placeholder{
    color: white !important;
   }
   button{
    background-color: dimgray !important;
    color: white !important;
    border-color: dimgray !important;
   }

   button :hover{
    background-color: rgb(150, 150, 150) !important;
   }

   .container{
    width: 450px;
   }

    .listas-enter-active,
    .listas-leave-active {
    transition: all 0.5s ease;
    }
    .listas-enter-from,
    .listas-leave-to {
    opacity: 0;
    transform: translateX(30px);
    }
   
</style>
