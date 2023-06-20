<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css'

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';
  let formData = {}; // Object to store form data

  // Define initial form field definitions
  let fieldDefinitions = [
    { id: 'fullname', label: 'Fullname', value: '' },
    { id: 'surname', label: 'Surname', value: '' },
    { id: 'email', label: 'Email', value: '' },
    { id: 'mobile', label: 'Mobile', value: '' },
    { id: 'address', label: 'Address/Location', value: '' },
    { id: 'languages', label: 'Languages', value: '' },
    { id: 'experience', label: 'Experience', value: '' },
    { id: 'dob', label: 'Date of Birth', value: '' },
    { id: 'rating', label: 'Rating', value: '' }
  ];

  function fetchData() {
    fetch(`https://api.recruitly.io/api/candidateform/details/${formId}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`)
      .then(response => response.json())
      .then(data => {
        console.log('API Response:', data);
        formFields = fieldDefinitions.map(field => ({
          ...field,
          value: data[field.id] || ''
        }));
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
    // Clear the formFields array before populating it
    formFields = [];

    // Push the form input values to the formFields array
    formFields = fieldDefinitions.map(field => ({
      ...field,
      value: formData[field.id] || ''
    }));

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
    {#each formFields as field}
      <div class="form-group">
        <label for={field.id} class="form-label">{field.label}</label>
        <input type="text" id={field.id} class="form-control" bind:value={formData[field.id]} />
      </div>
    {/each}

    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
