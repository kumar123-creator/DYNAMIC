<script>
    let formData = {
      private String name: '',
      private String email: '',
      private String phone: ''
    };
  
    let cvFile ={
        private String fileContent;
        private String fileName;
    };
     
    function handleSubmit() {
      const form = new FormData();
      form.append('name', formData.name);
      form.append('email', formData.email);
      form.append('phone', formData.phone);
      form.append('cv', cvFile);
  
      fetch('https://recruitly-dev.postman.co/workspace/Recruitly-Workspace~60cb4f48-0bbe-49e7-8a17-511240cebdd1/request/14566262-07d1281d-379f-41f1-a88c-ff6fb6e20d8c', {
        method: 'POST',
        body: form
      })
        .then(response => response.json())
        .then(data => {
          // Handle the response data
          console.log(data);
        })
        .catch(error => {
          // Handle any errors
          console.error(error);
        });
    }
  
    function handleCVUpload(event) {
      cvFile = event.target.files[0];
    }
  </script>
  
  <main>
    <form on:submit={handleSubmit}>
      <label for="name">Name</label>
      <input type="text" id="name" name="name" bind:value={formData.name} />
  
      <label for="email">Email</label>
      <input type="email" id="email" name="email" bind:value={formData.email} />
  
      <label for="phone">Phone</label>
      <input type="tel" id="phone" name="phone" bind:value={formData.phone} />
  
      <label for="cv">Upload CV</label>
      <input type="file" id="cv" name="cv" on:change={handleCVUpload} />
  
      <button type="submit">Submit</button>
    </form>
  </main>
  
  <style>
    main {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
  
    form {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
  
    label {
      margin-bottom: 0.5rem;
    }
  
    input {
      padding: 0.5rem;
      margin-bottom: 1rem;
      width: 300px;
    }
  
    button {
      padding: 0.5rem 1rem;
    }
  </style>
  
  
  
