<script>
  export let avatar
  export let name
  export let bio
  export let username
  export let jobTitle
  export let company
  export let pronoun
  export let location
  export let userBadges
</script>

<section class="bg-white border my-16 card shadow">
  <div class="px-10 pt-10">
    <div class="avatar">
      <div class="rounded-full h-32 mb-8 w-32">
        <img alt={avatar.altText} src={avatar.url} />
      </div>
    </div>
    <h2 class="font-bold text-left leading-relaxed text-2xl">
      {name}
    </h2>
    <ul class="flex space-x-2 text-left leading-relaxed opacity-75">
      <li>@{username}</li>
      <span class="opacity-75">&bull;</span>
      <li>{jobTitle},</li>
      <li>{company}</li>
      <span class="opacity-75">&bull;</span>
      <li>{pronoun}</li>
      <span class="opacity-75">&bull;</span>
      <li>{location}</li>
    </ul>
    <div>
      {#if userBadges}
        <div class="flex flex-wrap mt-5 break-words relative">
          {#each userBadges as { name }}
            <div
              class="border rounded-full font-medium mr-2 mb-2 py-2 px-4"
            >
              {name}
            </div>
          {/each}
        </div>
      {/if}
    </div>
    <div class="my-5 opacity-75">
      <p>
        {@html bio.split('\n').join('<br />')}
      </p>
    </div>
  </div>
</section>
