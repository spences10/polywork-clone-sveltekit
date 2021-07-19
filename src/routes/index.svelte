<script context="module">
  import Basics from '$lib/basics.svelte'
  import { format } from 'date-fns'
  import { gql, GraphQLClient } from 'graphql-request'

  export async function load() {
    const graphcms = new GraphQLClient(
      import.meta.env.VITE_CONTENT_API,
      {
        headers: {},
      }
    )
    const query = gql`
      {
        timelineUser(where: { username: "spences10" }) {
          id
          name
          username
          jobTitle
          company
          pronoun
          location
          bio
          avatar {
            url
            altText
          }
          banner {
            url
            altText
          }
          userBadges {
            name
          }
          activityDetails(orderBy: createdAt_DESC) {
            date
            description {
              html
            }
            activityTags {
              id
              name
              badgeColour {
                hex
              }
            }
          }
        }
      }
    `
    const { timelineUser } = await graphcms.request(query)
    return {
      props: {
        timelineUser,
      },
    }
  }
</script>

<script>
  export let timelineUser
  let {
    name,
    username,
    jobTitle,
    company,
    pronoun,
    location,
    bio,
    banner,
    avatar,
    userBadges,
    activityDetails,
  } = timelineUser
</script>

<img alt={banner.altText} src={banner.url} class="h-96 w-full" />

<!-- Containers -->
<article
  class="mx-auto -mt-72 max-w-7xl px-4 relative sm:px-6 lg:px-8"
>
  <div class="mx-auto max-w-3xl">
    <!-- Basics Card -->
    <Basics
      {avatar}
      {name}
      {bio}
      {username}
      {jobTitle}
      {company}
      {pronoun}
      {location}
      {userBadges}
    />

    <div class="divider opacity-10 mb-10" />

    <!-- Activities List -->
    {#if activityDetails}
      {#each activityDetails as activity}
        <section class="border mb-4 card shadow">
          <div class="card-body">
            {#if activity.activityTags}
              {#each activity.activityTags as { name, badgeColour }}
                <div class="flex flex-wrap break-words relative">
                  <div
                    class="border font-semibold mr-2 text-sm mb-2 w-auto py-2 px-4"
                  >
                    <p style="color:{badgeColour.hex}">{name}</p>
                  </div>
                </div>
              {/each}
            {/if}
            <p class="font-semibold mb-5 opacity-75">
              {format(new Date(activity.date), 'do MMM yyy')}
            </p>
            <div class="prose">
              {@html activity.description.html}
            </div>

            <div />
          </div>
        </section>
      {/each}
    {/if}
  </div>
</article>
