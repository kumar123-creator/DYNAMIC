<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css'

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';
  let fullname = '';
  let email = '';
  let mobile = '';
  let address = '';
  let languages = '';
  let experience = '';
  let dob = '';
  let rating = '';

  function fetchData() {
    fetch(`https://api.recruitly.io/api/candidateform/details/${formId}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`)
      .then(response => response.json())
      .then(data => {
        console.log('API Response:', data);
        fullname = data.fullname;
        email = data.email;
        mobile = data.mobile;
        address = data.address;
        languages = data.languages;
        experience = data.experience;
        dob = data.dob;
        rating = data.rating;

        // Update the form fields based on the fetched data
        formFields = [
          { label: 'Fullname', value: fullname },
          { label: 'Email', value: email },
          { label: 'Mobile', value: mobile },
          { label: 'Address', value: address },
          { label: 'Languages', value: languages },
          { label: 'Experience', value: experience },
          { label: 'Date of Birth', value: dob },
          { label: 'Rating', value: rating }
        ];
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
    // Clear the formFields array before populating it
    formFields = [];

    // Push the form input values to the formFields array
    formFields.push({ label: 'Fullname', value: fullname });
    formFields.push({ label: 'Email', value: email });
    formFields.push({ label: 'Mobile', value: mobile });
    formFields.push({ label: 'Address', value: address });
    formFields.push({ label: 'Languages', value: languages });
    formFields.push({ label: 'Experience', value: experience });
    formFields.push({ label: 'Date of Birth', value: dob });
    formFields.push({ label: 'Rating', value: rating });

    // Perform any necessary actions on form submission
    
    console.log('Form Fields:', formFields);
  }

  onMount(() => {
    // Add an event listener to form fields
    function handleFieldChange(event) {
      formId = event.target.value;
      fetchData(); // Fetch data when the form ID is changed
    }

    const fields = document.querySelectorAll('input[type="text"]');
    fields.forEach(field => field.addEventListener('input', handleFieldChange));
  });
</script>

<style>
  .form-container {
    max-width: 400px;
    margin: 0 auto;
  }

  .form-container label {
    margin-bottom: 0.5rem;
    color: #555;
    font-weight: bold;
  }

  .form-container input[type="text"] {
    border: 1px solid #ccc;
    padding: 0.5rem;
    width: 100%;
    border-radius: 4px;
  }

  .form-container button {
    margin-top: 1rem;
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
  }

  .form-container button:hover {
    background-color: #0056b3;
  }
</style>


<div class="form-container">
  <form on:submit|preventDefault={handleSubmit}>
    <div class="form-group">
      <label for="fullname" class="form-label">Fullname</label>
      <input type="text" id="fullname" class="form-control" bind:value={fullname} />
    </div>

    <div class="form-group">
      <label for="email" class="form-label">Email</label>
      <input type="text" id="email" class="form-control" bind:value={email} />
    </div>

    <div class="form-group">
      <label for="mobile" class="form-label">Mobile</label>
      <input type="text" id="mobile" class="form-control" bind:value={mobile} />
    </div>
    
    <div class="form-group">
      <label for="address" class="form-label">Address/Location</label>
      <input type="text" id="address" class="form-control" bind:value={address} />
    </div>
    
    <div class="form-group">
      <label for="languages" class="form-label">Languages</label>
      <input type="text" id="languages" class="form-control" bind:value={languages} />
    </div>
    
    <div class="form-group">
      <label for="experience" class="form-label">Experience</label>
      <input type="text" id="experience" class="form-control" bind:value={experience} />
    </div>
    
    <div class="form-group">
      <label for="dob" class="form-label">Date of Birth</label>
      <input type="text" id="dob" class="form-control" bind:value={dob} />
    </div>
    
    <div class="form-group">
      <label for="rating" class="form-label">Rating</label>
      <input type="text" id="rating" class="form-control" bind:value={rating} />
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
