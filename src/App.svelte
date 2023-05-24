
  <script>
  let fullName = '';
  let email = '';
  let mobile = '';
  let cvFile = null;
  let fileContent = '';
  let fileName = '';

  async function handleFileChange(event) {
    cvFile = event.target.files[0];
    fileContent = await toBase64(cvFile);
    fileName = cvFile.name;
  }

  async function uploadCV() {
    if (validateForm()) {
      const postData = {
        fullName,
        email,
        mobile,
        fileContent,
        fileName
      };

      try {
        const response = await fetch(
          'https://api.recruitly.io/api/cvsubmit/bytes?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77',
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(postData)
          }
        );

        // Handle the response as needed
        if (response.ok) {
          console.log('CV uploaded successfully');
          resetForm();
          location.reload();
        } else {
          console.error('Failed to upload CV');
        }
      } catch (error) {
        console.error('Error uploading CV:', error);
      }
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

  function validateForm() {
    if (!fullName || !email || !mobile || !cvFile) {
      console.error('Please fill in all the fields and select a CV file');
      return false;
    }

    // Add additional validation if needed

    return true;
  }

  function resetForm() {
    fullName = '';
    email = '';
    mobile = '';
    cvFile = null;
    fileContent = '';
    fileName = '';
  }
</script>

<style>
  @import url('https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css');
</style>

<main>
  <h1>CANDIDATE FORM</h1>

  <div class="form-group">
    <label for="fullName">Full Name:</label>
    <input class="form-control" type="text" id="fullName" bind:value={fullName} placeholder="Enter your full name" />
  </div>

  <div class="form-group">
    <label for="email">Email:</label>
    <input class="form-control" type="email" id="email" bind:value={email} placeholder="Enter your email address" />
  </div>

  <div class="form-group">
    <label for="mobile">Mobile:</label>
    <input class="form-control" type="tel" id="mobile" bind:value={mobile} placeholder="Enter your mobile number" />
  </div>

  <div class="form-group">
    <label for="cvFile">CV File:</label>
    <input class="form-control-file" type="file" accept=".pdf,.doc,.docx" id="cvFile" on:change={handleFileChange} />
  </div>

  <button class="btn btn-primary" on:click={uploadCV}>Submit</button>
</main>
