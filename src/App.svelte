<script>
  import { onMount } from 'svelte';

  let candidates = [];

  // Function to add a new candidate
  function addCandidate() {
    candidates = [...candidates, { name: '', email: '', phone: '' }];
  }

  // Function to remove a candidate
  function removeCandidate(index) {
    candidates = candidates.filter((_, i) => i !== index);
  }

  // Lifecycle hook to add an initial candidate
  onMount(async () => {
    try {
      const response = await fetch(
        'https://api.recruitly.io/api/candidateform/details/72fbc0da-3810-4ad9-a922-1845f8974eb7?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77'
      );
      const data = await response.json();
      candidates = data.candidates.map(candidate => ({ ...candidate }));
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  });
</script>

<style>
  .container {
    padding: 2rem;
  }

  .card {
    margin-bottom: 1rem;
  }

  .card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.75rem 1.25rem;
    background-color: #f8f9fa;
    border-bottom: 1px solid rgba(0, 0, 0, 0.125);
  }

  .card-body {
    padding: 1rem;
  }

  .btn {
    margin-right: 0.5rem;
  }
</style>

<div class="container">
  <h1>Dynamic Candidate Form</h1>

  {#each candidates as candidate, index}
  <div class="card">
    <div class="card-header">
      <h5 class="card-title">Candidate {index + 1}</h5>
      {#if index > 0}
      <button class="btn btn-danger btn-sm" on:click={() => removeCandidate(index)}>
        Remove
      </button>
      {/if}
    </div>
    <div class="card-body">
      <div class="form-group">
        <label for="name">Name</label>
        <input type="text" class="form-control" id="name" bind:value={candidate.name} required>
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" class="form-control" id="email" bind:value={candidate.email} required>
      </div>

      <div class="form-group">
        <label for="phone">Phone</label>
        <input type="tel" class="form-control" id="phone" bind:value={candidate.phone} required>
      </div>
    </div>
  </div>
  {/each}

  <button class="btn btn-primary" on:click={addCandidate}>Add Candidate</button>
</div>
