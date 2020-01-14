<template>
    <div>
        <b-button @click="createItem" class="mb-2" variant="primary" size="sm">Create</b-button>

        <b-table striped hover  :items="tableData" :fields="columns">
            <template #cell(action)="data">
                <b-button @click="editItem(data.item)" variant="primary" size="sm">Edit</b-button>
                <b-button @click="deleteItem(data.item)" v-b-modal="'edit-modal'" variant="danger" size="sm">Delete</b-button>
            </template>
        </b-table>

        <b-modal v-model="modalShow" :title="formTitle" hide-footer>
            <b-form @submit.prevent="save">
                <slot :formdata="editedItem" name="input-fields">
                </slot>

                <b-button size="sm" variant="danger" @click="close" >
                    Cancel
                </b-button>

                <b-button type="submit" size="sm" variant="success">
                    Submit
                </b-button>
            </b-form>

        </b-modal>
    </div>
</template>

<script>
    export default {
        props:['endpoint','columns','formFields'],
        data() {
            return {
                editedItem: this.formFields,
                modalShow:false,
                editedIndex: -1,
                tableData : [],
            }
        },
        computed: {
            formTitle() {
                return this.editedIndex === -1 ? 'New Item' : 'Edit Item';
            },
        },

        methods: {
            createItem() {
                this.modalShow = true;
                this.editedItem = Object.assign({}, this.formFields);
                this.editedIndex = -1;
            },
            editItem(item) {
                this.modalShow = true;
                this.editedIndex = this.tableData.indexOf(item);

                this.editedItem = Object.assign({}, item);
            },

            deleteItem(item) {
                const index = this.tableData.indexOf(item);
                confirm('Are you sure you want to delete this item?') && this.tableData.splice(index, 1);

                axios.delete(this.endpoint+'/'+item.id);

            },
            close() {
                this.modalShow = false;
                setTimeout(() => {
                    this.editedItem = Object.assign({}, this.formFields);
                    this.editedIndex = -1;
                }, 300);
            },
            save() {
                if (this.editedIndex > -1) {
                    Object.assign(this.tableData[this.editedIndex], this.editedItem);
                    axios.put(this.endpoint+'/'+this.editedItem.id,this.editedItem);
                } else {
                    this.tableData.push(this.editedItem);
                    axios.post(this.endpoint,this.editedItem);
                }
                this.close();
            }
        },
        created() {
            axios(this.endpoint).then(response=>this.tableData = response.data.data);
        }
    }
</script>