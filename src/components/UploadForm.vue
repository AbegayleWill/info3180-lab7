<template>
    <form @submit.prevent="uploadPhoto" id ="uploadForm" action ="" method = "Post">

        <div class="form-group">
            <label for="movie-title">Description</label>
            <textarea  name="description" id="description" class="form-control"></textarea>
        </div>
        <div class = "form-group">
            <label for="file">Photo Upload</label>
            <input type="file" name="photo" id="file" class="form-control" />      
        </div>
        <div class = "form-group">
            <button type ="submit" class="btn btn-primary mb-2">Submit</button>
        </div>
            
    </form>

</template>

<script>

export default {
    data() {
        
        return {csrf_token: ''}
         
    },
    created() {
        this.getCsrfToken();
    },
    methods:{
        
        uploadPhoto() {
            let uploadForm = document.getElementById('uploadForm');
            let form_data = new FormData(uploadForm);

            fetch("/api/upload", {
                method: 'POST',
                body: form_data,
                headers: {
                    'X-CSRFToken': this.csrf_token
                }
            })
                .then(function (response) {
                return response.json();
                })
                .then(function (data) {
                // display a success message
                console.log(data);
                })
                .catch(function (error) {
                console.log(error);
                });
    },
    getCsrfToken() {
        let self = this;
        fetch('/api/csrf-token')
        .then((response) => response.json())
        .then((data) => {
        console.log(data);
        self.csrf_token = data.csrf_token;
        })
    }

    }
}
</script>

<style>

</style>