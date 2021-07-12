<script context="module">
  import { gql, GraphQLClient } from 'graphql-request'
  export async function load(context) {
    const graphcms = new GraphQLClient(
      import.meta.env.VITE_CONTENT_API,
      {
        headers: {},
      }
    )
    const query = gql`
      {
        polyworkUser(where: { username: "spences10" }) {
          id
          name
          bio
          polyworkUserBadges {
            name
          }
          activityDetails {
            date
            description {
              html
            }
            activityTags {
              id
              name
            }
          }
        }
      }
    `
    const variables = {
      slug: context.page.params.slug,
    }
    const { polyworkUser } = await graphcms.request(query, variables)
    return {
      props: {
        polyworkUser,
      },
    }
  }
</script>

<script>
  export let polyworkUser
  let {
    name,
    bio,
    polyworkUserBadges,
    activityDetails,
  } = polyworkUser
</script>

<p>{name}</p>
<article>{bio}</article>
{#if polyworkUserBadges}
  <ul>
    {#each polyworkUserBadges as { name }}
      <li>{name}</li>
    {/each}
  </ul>
{/if}

{#if activityDetails}
  <section>
    {#each activityDetails as activity}
      <p>{activity.date}</p>
      <div>{@html activity.description.html}</div>
      {#if activity.activityTags}
        {#each activity.activityTags as { name }}
          <p>{name}</p>
        {/each}
      {/if}
      <div />
    {/each}
  </section>
{/if}

<div class="container bg-gray-200 mx-auto w-full h-full">
  <div class="relative wrap overflow-hidden p-10 h-full">
    <div
      class="border-2-2 absolute border-dashed border-blue-500 h-full border"
      style="left: 5.8%"
    />

    <!-- First timeline -->
    <div class="mb-8 flex justify-between items-center w-full">
      <div class="order-2 w-6/12" />
      <div class="z-20">
        <div
          class="my-4 rounded-full h-10 w-10 flex items-center bg-indigo-300 ring-4 ring-indigo-400 ring-opacity-30"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-10 w-10 text-green-600"
            viewBox="0 0 20 20"
            fill="currentColor"
          >
            <path
              fill-rule="evenodd"
              d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
              clip-rule="evenodd"
            />
          </svg>
        </div>
      </div>
      <div
        class="order-1 bg-gray-300 rounded-lg shadow-xl w-5/12 px-6 py-4"
      >
        <div class="flex flex-row">
          <h3 class="mb-3 font-bold text-gray-800 text-xl">Status</h3>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            viewBox="0 0 20 20"
            fill="red"
          >
            <path
              fill-rule="evenodd"
              d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z"
              clip-rule="evenodd"
            />
          </svg>
          <h5 class="mb-3 font-bold text-gray-800 text-xl">Loc</h5>
        </div>

        <p
          class="text-base leading-snug tracking-wide text-gray-900 text-opacity-100"
        >
          statusinfo helooooooooooooooooooooooo
        </p>
      </div>
    </div>

    <!-- Second timeline -->
    <div class="mb-8 flex justify-between items-center w-full">
      <div class="order-2 w-6/12" />
      <div class="z-20">
        <div
          class="my-4 rounded-full h-10 w-10 flex items-center bg-indigo-300 ring-4 ring-indigo-400 ring-opacity-30"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-10 w-10 text-green-600"
            viewBox="0 0 20 20"
            fill="currentColor"
          >
            <path
              fill-rule="evenodd"
              d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
              clip-rule="evenodd"
            />
          </svg>
        </div>
      </div>
      <div
        class="order-1 bg-red-200 rounded-lg shadow-xl w-5/12 px-6 py-4"
      >
        <div class="flex flex-row">
          <h3 class="mb-3 font-bold text-gray-800 text-xl">Status</h3>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            viewBox="0 0 20 20"
            fill="red"
          >
            <path
              fill-rule="evenodd"
              d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z"
              clip-rule="evenodd"
            />
          </svg>
          <h5 class="mb-3 font-bold text-gray-800 text-xl">Loc</h5>
        </div>

        <p
          class="text-base leading-snug tracking-wide text-gray-900 text-opacity-100"
        >
          statusinfo helooooooooooooooooooooooo
        </p>
      </div>
    </div>

    <!-- you can add more time line from here :) -->
  </div>
</div>
