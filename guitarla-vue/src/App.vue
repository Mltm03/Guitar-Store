<script setup> // con esto ya usas compositionAPI
import {ref,reactive, onMounted, watch} from 'vue'
import {db} from './data/guitarras'
import Guitarra from  "./components/Guitarra.vue"
import Header from "./components/Header.vue"
import Footer from "./components/Footer.vue"

//utilizando reactive para el array de guiitarras
// const state=reactive({
//     guitarras:[]
// })
// console.log(state.guitarras)
///-------------------

///utilizando ref para manejar las guitarras
const guitarras=ref([])
const carrito=ref([])
const guitarraP=ref({})
//--------------
watch(carrito, ()=>{
    guardarLocal()
}, {deep:true})

onMounted(()=>{
    guitarras.value=db
    guitarraP.value=db[3]

    const carritoStorage = localStorage.getItem('carrito')
    if (carritoStorage){
        carrito.value=JSON.parse(carritoStorage)
    }
})

const guardarLocal=()=> {
    localStorage.setItem('carrito', JSON.stringify(carrito.value));
}   
const agregarCarrito =(object)=>{
        const existeCarrito= (carrito.value.findIndex(producto =>  producto.id === object.id))
        if (existeCarrito  >=0){ 
            carrito.value[existeCarrito].cantidad++  
        }else{
            object.cantidad= 1; 
            carrito.value.push(object); 
        }
        
    }
const aumentarProducto =(carritos)=>{
    if(carritos.cantidad >= 5 ) return 
    carritos.cantidad++
}
const decrementarProducto =(carritos)=>{
       if(carritos.cantidad <= 1) return 
       carritos.cantidad--
   }
const eliminarProducto =(carritos)=>{
    carrito.value= carrito.value.filter(producto =>  producto.id !== carritos.id)
    
    }
const vaciarCarrito =()=> carrito.value.length=0
</script>

<template>

    <Header
    
    :carrito="carrito"
    :guitarraP="guitarraP"
    @eliminar-producto="eliminarProducto"
    @aumentar-producto="aumentarProducto"
    @decrementar-producto="decrementarProducto"
    @agregar-carrito="agregarCarrito"
    @vaciar-carrito="vaciarCarrito"
    />
        
  
  

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
          
            <Guitarra 
                v-for="guitarra  in guitarras"
                :guitarra="guitarra"

                @agregar-carrito="agregarCarrito"
            />
            
        </div>
    </main>

    <Footer></Footer>
   

</template>

