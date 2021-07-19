<script context="module">
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
<div class="mx-auto -mt-72 max-w-7xl px-4 relative sm:px-6 lg:px-8">
  <div class="mx-auto max-w-3xl">
    <!-- Basics Card -->
    <div class="bg-white border my-16 card shadow">
      <div class="">
        <figure class="px-10 pt-10">
          <div class="avatar">
            <div class="rounded-full h-32 mb-8 w-32">
              <img alt={avatar.altText} src={avatar.url} />
            </div>
          </div>
          <h2 class="font-bold text-left leading-relaxed text-2xl">
            {name}
          </h2>
          <ul
            class="flex space-x-2 text-left leading-relaxed opacity-75"
          >
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
        </figure>
      </div>
    </div>

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
</div>
