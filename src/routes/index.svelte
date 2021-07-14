<script context="module">
  import { format } from 'date-fns'
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
          polyworkUserBadges {
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
    username,
    jobTitle,
    company,
    pronoun,
    location,
    bio,
    banner,
    avatar,
    polyworkUserBadges,
    activityDetails,
  } = polyworkUser
</script>

<img alt={banner.altText} src={banner.url} class="h-96 w-full" />

<div class="mx-auto -mt-72 max-w-7xl px-4 relative sm:px-6 lg:px-8">
  <div class="mx-auto max-w-3xl">
    <!-- Basics Card -->
    <div class="bg-white my-16 card border">
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
            <li>{jobTitle},</li>
            <li>{company}</li>
            <li>{pronoun}</li>
            <li>{location}</li>
          </ul>
          <div>
            {#if polyworkUserBadges}
              <div class="flex flex-wrap break-words mt-5 relative">
                {#each polyworkUserBadges as { name }}
                  <div
                    class="border rounded-full py-2 px-4 mr-2 mb-2 font-medium"
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

    <hr />

    <!-- Activities List -->
    {#if activityDetails}
      {#each activityDetails as activity}
        <section class="mb-4 mt-16 card border">
          <div class="card-body">
            {#if activity.activityTags}
              {#each activity.activityTags as { name }}
                <div class="flex flex-wrap break-words relative bg-">
                  <div
                    class="border py-2 px-4 mr-2 mb-2 text-sm font-semibold w-auto"
                  >
                    <p>{name}</p>
                  </div>
                </div>
              {/each}
            {/if}
            <p>{format(new Date(activity.date), 'do MMM yyy')}</p>
            <div>{@html activity.description.html}</div>

            <div />
          </div>
        </section>
      {/each}
    {/if}
  </div>
</div>
