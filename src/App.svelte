<script>
  import { onMount } from 'svelte';

  let formFields = [];

  async function fetchFormFields() {
    try {
      const response = await fetch('https://api.recruitly.io/api/job?apiKey=TEST64518616D4CF145D4E20BD47169EA7229BA3');
      formFields = await response.json();
    } catch (error) {
      console.error(error);
    }
  }

  async function handleSubmit(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    const formValues = {};

    for (let [key, value] of formData.entries()) {
      formValues[key] = value;
    }

    try {
      const response = await fetch(https://api.recruitly.io/api/job?apiKey=TEST64518616D4CF145D4E20BD47169EA7229BA3', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(formValues)
      });

      if (response.ok) {
        // Handle successful form submission
        console.log('Form submitted successfully!');
      } else {
        // Handle form submission error
        console.error('Error submitting form');
      }
    } catch (error) {
      console.error(error);
    }
  }

  onMount(fetchFormFields);
</script>

<main>
  {#if formFields.length > 0}
    <form on:submit={handleSubmit}>
      {#each formFields as field}
        <label for={field.name}>{field.label}</label>
        {#if field.type === 'text'}
          <input type="text" id={field.name} name={field.name} />
        {:else if field.type === 'email'}
          <input type="email" id={field.name} name={field.name} />
        {:else if field.type === 'number'}
          <input type="number" id={field.name} name={field.name} />
        {:else}
          <!-- Handle other field types here -->
        {/if}
      {/each}
      <button type="submit">Submit</button>
    </form>
  {:else}
    <p>Loading form fields...</p>
  {/if}
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
