<template>
    <div>
        <h1 class="text-center">Gestion del Personal</h1>
        <hr />

        <!-- Button trigger modal -->
        <button type="button" @click="modificar=false; abrirModal();" class="btn btn-info">
        Agregar un nuevo Personal
        </button>

        <!-- Modal -->
        <div class="modal" :class="{mostrarModal:modal}">
            <div class="modal-dialog">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel"> {{modalTitulo}} </h5>
                    <button type="button" class="btn-close" aria-label="Close" @click="cerrarModal();"></button>
                </div>
                <div class="modal-body">
                    <div>
                        <label for="nombre">Nombre</label>
                        <input  v-model="form.nombre" type="text" name="nombre" class="form-control" placeholder="Ingresar Nombre.." id="">
                    </div>
                    <div>
                        <label for="nombre">Apellido</label>
                        <input v-model="form.apellido" type="text" name="apellido" class="form-control" placeholder="Ingresar Apellido.." id="">
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" @click="cerrarModal();">Close</button>
                    <button type="button" class="btn btn-success" @click="guardar();">Guardar</button>
                </div>
                </div>
            </div>
        </div>
        <hr />
        <table class="table table-striped">
            <thead>
                <tr>
                <th scope="col">#</th>
                <th scope="col">Nombre</th>
                <th scope="col">Apellido</th>
                <th scope="col" colspan="2" class="text-center">Estado</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item of personal" :key="item.id">
                    <th scope="row" > {{item.id}} </th>
                    <td> {{item.nombre}} </td>
                    <td> {{item.apellido}} </td>
                    <td>
                        <button type="button" @click="modificar=true; abrirModal(item);" class="btn btn-warning">Editar</button>
                    </td>
                    <td>
                        <button type="button" class="btn btn-danger" @click="eliminar(item.id)">Eliminar</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                nombre:'ejemplo',
                apellido:'ejempl'
            },
            idPersonal: 0,
            modificar: true,
            modal: 0,
            modalTitulo: '',
            personal: [],
        }
    },
    methods: {
        async ver(){
            const res = await axios.get('/personal');
            this.personal = res.data; 
        },
        async eliminar(id){
            const res = await axios.delete('/personal/'+id);
            this.ver(); 
        },
        async guardar(){
            if (this.modificar) {
                const res = await axios.put('/personal/'+this.idPersonal,this.form);
            }else{
                const res = await axios.post('/personal',this.form);
            }
            this.cerrarModal();
            this.ver(); 
        },
        abrirModal(data ={}){
            this.modal = 1;
            if (this.modificar) {
                this.idPersonal = data.id
                this.modalTitulo = 'Modificar Personal';
                this.form.nombre = data.nombre;
                this.form.apellido = data.apellido;
            }else{
                this.idPersonal = 0;
                this.modalTitulo = 'Crear Personal';
                this.form.nombre = '';
                this.form.apellido = '';
            }
        },
        cerrarModal(){
            this.modal = 0;
        }
    },
    created(){
        this.ver();
    }
}
</script>
<style>
    .mostrarModal{
        display: list-item;
        opacity: 100%;
        background: rgba(45, 56, 95, 0.842);
    }
</style>