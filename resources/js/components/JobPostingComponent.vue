<template>
    <div>
        <h1>Job Posting List</h1>
        <table class='table table-condensed table-border'>
            <thead>
                <th>Job Name</th>
                <th>Job Description</th>
                <th>Vacants</th>
                <th>Actions</th>
            </thead>
            <tbody>
                <tr v-for="(item, index) in postingList" :key="item.id">
                    <td>{{ item.job_name }}</td>
                    <td>{{ item.job_description }}</td>
                    <td>{{ item.job_vacants }}</td>
                    <td>
                        <button class="btn btn-secondary btn-sm" @click="edit()"><i class="fa fa-edit"></i>Edit</button>
                        <button class="btn btn-danger btn-sm" @click="remove(item, index)"><i class="fa fa-trash"></i>Del</button>
                    </td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td>
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="Input Job Name" v-model="form.job_name">
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="Input Job Description" v-model="form.job_description">
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <input type="number" class="form-control" placeholder="Input Job Vacant" v-model="form.job_vacants">
                        </div>
                    </td>
                    <td>
                        <div class="form-group">
                            <button class="btn btn-primary" @click="submit"><i class="fas fa-plus"></i>Add</button>
                        </div>
                    </td>
                </tr>
            </tfoot>
        </table>

        <!-- Modal -->
        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Modal title</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
            </div>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['posting'],
    data(){
        return{
            postingList: this.posting,
            form: {
                job_name: null,
                job_description: null,
                job_vacants: 0
            }
        }
    },
    methods: {
        submit() {
            const vm = this;
            // alert('testing');
            axios.post('/job_postings', this.form)
            .then(function (response) {
                // alert('success');
                vm.postingList.push(response.data.data);
                vm.form.job_name = null
                vm.form.job_description = null
                vm.form.job_vacants = 0
            })
            .catch(function (error) {
                // console.log(error);
                alert('Something went wrong.');
            });
        },
        remove(item, index) {
            const vm = this;
            axios.delete(`/job_postings/${item.id}`)
            .then(function (response) {
                // alert('success');
                vm.postingList.splice(index, 1);
            })
            .catch(function (error) {
                // console.log(error);
                alert('Something went wrong.');
            });
        },
        edit() {
             $('#editModal').modal('show');
        }
    }
}
</script>