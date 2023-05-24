<script>
  import 'bootstrap/dist/css/bootstrap.min.css';

  let formData = {
    name: '',
    email: '',
    mobile: ''
  };
  
  let cvFile = null;

  function handleFileChange(event) {
    cvFile = event.target.files[0];
  }

  async function uploadCV() {
    if (cvFile) {
      const base64Data = await toBase64(cvFile);
      const postData = {
        fileContent: base64Data
      };

      const response = await fetch('https://api.recruitly.io/api/cvsubmit/bytes?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(postData)
      });

      // Handle the response as needed
    }
  }

  function toBase64(file) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => resolve(reader.result.split(',')[1]);
      reader.onerror = error => reject(error);
    });
  }

  function handleSubmit() {
    const jsonData = {
      ...formData,
      cv: cvFile
    };

    const formData = new FormData();
    formData.append('data', JSON.stringify(jsonData));
    formData.append('cv', cvFile);

    fetch('https://api.recruitly.io/api/cvsubmit/bytes?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77', {
      method: 'POST',
      body: formData
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
    <div class="form-group">
      <label for="name">Name</label>
      <input type="text" id="name" name="name" class="form-control" bind:value={formData.name} />
    </div>

    <div class="form-group">
      <label for="email">Email</label>
      <input type="email" id="email" name="email" class="form-control" bind:value={formData.email} />
    </div>

    <div class="form-group">
      <label for="mobile">Mobile</label>
      <input type="tel" id="Mobile" name="mobile" class="form-control" bind:value={formData.mobile} />
    </div>

    <div class="form-group">
      <label for="cv">Upload CV</label>
      <input type="file" id="cv" name="cv" class="form-control-file" on:change={handleCVUpload} />
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
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
    width: 300px;
  }

  label {
    margin-bottom: 0.5rem;
  }

  input {
    margin-bottom: 1rem;
  }

  .form-group {
    margin-bottom: 1.5rem;
  }

  .btn {
    padding: 0.5rem 1rem;
  }
</style>
``
