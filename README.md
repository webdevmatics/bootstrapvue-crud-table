# Bootstrapvue-crud-table

### Reusable vue component that creates CRUD table


### Usage
1. Download Component and put it in your project, Use it in any component. I have used it in permission component as displayed below. 

 ```vue
<template>
    <div>
        <h3>Permissions</h3>
        <crud-table
                endpoint="/permissions"
                :columns="['name','action']"
                :form-fields="{
                name: ''
            }"
        >
            <!-- your form input fields in this slot-->
            <template v-slot:input-fields="{formdata}">
                <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
                    <b-form-input
                            id="input-2"
                            v-model="formdata.name"
                            required
                            placeholder="Enter name"
                    ></b-form-input>
                </b-form-group>
            </template>
        </crud-table>

    </div>
</template>

<script>
    import CrudTable from '../components/crud-table';

    export default {
        components: {CrudTable},
    };
</script>

<style scoped>

</style>
  ```
