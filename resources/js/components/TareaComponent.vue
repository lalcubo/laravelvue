<template>
    <div>
        <h3>Agregar Notas</h3>
        <form @submit.prevent="agregar">
            <input type="text" placeholder="Nombre" class="form-control mb-2" v-model="nota.nombre">
            <input type="text" placeholder="Descripción" class="form-control mb-2" v-model="nota.descripcion">
            <button class="btn btn-primary" type="submit">Agregar</button>
        </form>
        <hr class="mt-3">
        <h3>Listado de notas</h3>
        <ul class="list-group my-2">
            <li class="list-group-item" 
            v-for="(item, index) in notas" :key="index">
                <span class="badge badge-primary float-right">
                    {{item.updated_at}}
                </span>
                <p>{{ item.nombre }}</p>
                <p>{{ item.descripcion }}</p>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data(){
        return{
            notas: [],
            nota: {nombre: '', descripcion:''}
        }
    },
    created(){
        axios.get('../public/notas')
        .then(res => {
            this.notas = res.data;
        })
    },
    methods:{
        agregar(){
            
            if(this.nota.nombre.trim() === '' || this.nota.descripcion.trim() === ''){
                alert('Debes completar todos los campos antes de guardar');
                return;
            }
            //console.log(this.nota.nombre, this.nota.descripcion);
            const params = {
                nombre: this.nota.nombre,
                descripcion: this.nota.descripcion
            }
            this.nota.nombre = '';
            this.nota.descripcion = '';
            axios.post('../public/notas',params)
            .then(res => {
                this.notas.push(res.data)   
            })
        }
    }
}
</script>