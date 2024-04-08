<script setup>
import {computed} from 'vue'

    const props= defineProps({
        carrito: {
            type:Array,
            required:true
        },
        guitarraP:{
            type:Object,
            required:true
        }
    })

    const totalPagar= computed(()=>{
        return  props.carrito.reduce( (total,producto)=> total + (producto.cantidad * producto.precio),0 )
    })
    defineEmits(['eliminar-producto', 'aumentar-producto', 'decrementar-producto', 
    'agregar-carrito', 'vaciar-carrito'])
</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div
                        class="carrito"
                    >
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                        <!-- Para hacer una condicion si el carrito esta vacio -->
                            <p v-if="carrito.length===0" 
                            class="text-center">El carrito esta vacio
                        
                            </p>
                        <div v-else>
                            <table  class="w-100 table">
                                <thead>
                                    <tr>
                                        <th>Imagen</th>
                                        <th>Nombre</th>
                                        <th>Precio</th>
                                        <th>Cantidad</th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <!-- iteramos sobre la tabla para  mostrar los productos del carrito  -->
                                    <tr 
                                         v-for="carritos in carrito"  
                                    >
                                        <td>
                                            <img class="img-fluid" 
                                                :src="'/img/'+carritos.imagen+ '.jpg' " 
                                                :alt="'imagen guitarra' + carritos.nombre"
                                                >
                                            
                                        </td>
                                        <td>{{carritos.nombre}}</td>
                                        <td class="fw-bold">
                                                ${{carritos.precio}}
                                        </td>
                                        <td class="flex align-items-start gap-4">
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                v-on:click="$emit('decrementar-producto', carritos)"
                                            >
                                                -
                                            </button>
                                                {{carritos.cantidad}}
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                v-on:click="$emit('aumentar-producto', carritos)"
                                            >
                                                +
                                            </button>
                                        </td>
                                        <td>
                                            <button
                                                class="btn btn-danger"
                                                type="button"
                                                v-on:click="$emit('eliminar-producto', carritos)"
                                            >
                                                X
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>

                            <p class="text-end">Total pagar: <span class="fw-bold">${{totalPagar}}</span></p>
                            <button class="btn btn-dark w-100 mt-3 p-2"
                            v-on:click="$emit('vaciar-carrito')"
                            >
                            Vaciar Carrito
                            </button>
                        </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{guitarraP.nombre}}</h1>
                    <p class="mt-5 fs-5 text-white">{{guitarraP.descripcion}}</p>
                    <p class="text-primary fs-1 fw-black">${{guitarraP.precio}}</p>
                    <button
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        v-on:click="$emit('agregar-carrito', guitarraP)"
                    >
                     Agregar al Carrito
                    </button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>
