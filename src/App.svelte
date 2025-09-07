<script>
  import { onMount } from 'svelte';

  let students = [];
  let error_students = '';

  onMount(async () => {
    try {
      const res = await fetch('/api/students');
      if (!res.ok) throw new Error(await res.text());
      students = await res.json();
    } catch (err) {
      error_students = err.message;
    }
  });

  let guardians = [];
  let error_guardians = '';

  onMount(async () => {
    try {
      const res = await fetch('/api/guardians');
      if (!res.ok) throw new Error(await res.text());
      guardians = await res.json();
    } catch (err) {
      error_guardians = err.message;
    }
  });

  let authorizations = [];
  let error_authorizations = '';

  onMount(async () => {
    try {
      const res = await fetch('/api/authorizations');
      if (!res.ok) throw new Error(await res.text());
      authorizations = await res.json();
    } catch (err) {
      error_authorizations = err.message;
    }
  });
  
</script>

{#if error_students}
  <p class="error">{error_students}</p>
{:else}
  <br/>
  <h3>Estudiantes</h3>
  <ul>
    {#each students as s}
      <li>{s.name} ({s.id})</li>
    {/each}
  </ul>
{/if}

{#if error_guardians}
  <p class="error">{error_guardians}</p>
{:else}
  <br/>
  <h3>Representantes Legales</h3>
  <ul>
    {#each guardians as g}
      <li>{g.name} ({g.id})</li>
    {/each}
  </ul>
{/if}

{#if error_authorizations}
  <p class="error">{error_authorizations}</p>
{:else}
  <br/>
  <h3>Autorizaciones</h3>
  <ul>
    {#each authorizations as a}
      <li>{a.title} ({a.id})</li>
    {/each}
  </ul>
{/if}