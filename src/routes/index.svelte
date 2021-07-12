<script context="module">
	import { gql, GraphQLClient } from 'graphql-request';
	export async function load(context) {
		const graphcms = new GraphQLClient(import.meta.env.VITE_CONTENT_API, {
			headers: {}
		});
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
		`;
		const variables = {
			slug: context.page.params.slug
		};
		const { polyworkUser } = await graphcms.request(query, variables);
		return {
			props: {
				polyworkUser
			}
		};
	}
</script>

<script>
	export let polyworkUser;
	let { name, bio, polyworkUserBadges, activityDetails } = polyworkUser;
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

