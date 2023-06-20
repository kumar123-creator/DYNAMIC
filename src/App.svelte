<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css'

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';

  function fetchData() {
    fetch(`https://api.recruitly.io/api/candidateform/details/${formId}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`)
      .then(response => response.json())
      .then(data => {
        console.log('API Response:', data);
        formFields = Object.entries(data).map(([key, value]) => ({ label: key, value }));
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
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

  function addFormField() {
    formFields = [...formFields, { label: '', value: '' }];
  }

  function removeFormField(index) {
    formFields = formFields.filter((_, i) => i !== index);
  }
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

  .form-container .add-field-button {
    margin-top: 0.5rem;
    background-color: #28a745;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    color: #fff;
    border: none;
    cursor: pointer;
  }

  .form-container .remove-field-button {
    margin-top: 0.5rem;
    background-color: #dc3545;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    color: #fff;
    border: none;
    cursor: pointer;
  }
</style>

<div class="form-container">
  <form on:submit|preventDefault={handleSubmit}>
    {#each formFields as field, index}
      <div class="form-group">
        <label for={`field-${index}`} class="form-label">Field {index + 1}</label>
        <input type="text" id={`field-${index}`} class="form-control" bind:value={field.value} />
      </div>
    {/each}

    <div class="form-group">
      <button type="submit" class="btn btn-primary">Submit</button>
    </div>
  </form>

  <div class="form-group">
    <button class="add-field-button" on:click={addFormField}>Add Field</button>
  </div>
  <div class="form-group">
    <button class="remove-field-button" on:click={removeFormField}>Remove Field</button>
  </div>
</div>
