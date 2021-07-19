<script context="module">
  import Activities from '$lib/activities.svelte'
  import Basics from '$lib/basics.svelte'
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
    <Activities {activityDetails} />
  </div>
</article>
