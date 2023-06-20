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
        formFields = Object.entries(data).map(([key, value]) => ({
          label: key.charAt(0).toUpperCase() + key.slice(1),
          value: value
        }));
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
    // Perform any necessary actions on form submission
    console.log('Form Fields:', formFields);
  }

  function addField() {
    formFields.push({ label: '', value: '' });
  }

  function removeField(index) {
    formFields.splice(index, 1);
  }

  onMount(() => {
    fetchData(); // Fetch data on component mount
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
    {#each formFields as field, index}
      <div class="form-group">
        <label for={index} class="form-label">{field.label}</label>
        <input type="text" id={index} class="form-control" bind:value={field.value} />
        {#if index === formFields.length - 1}
          <button type="button" class="btn btn-danger" on:click={() => removeField(index)}>Remove</button>
        {/if}
      </div>
    {/each}

    <button type="button" class="btn btn-success" on:click={addField}>Add Field</button>

    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
