<script>
  import 'bootstrap/dist/css/bootstrap.min.css';
  
  let formData = {
    name: '',
    email: '',
    phone: ''
  };

  let cvFile;

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
