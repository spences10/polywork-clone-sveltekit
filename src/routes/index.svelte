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
					avatar {
						url
					}
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
	let { name, bio, avatar, polyworkUserBadges, activityDetails } = polyworkUser;
</script>

<div class="avatar">
	<div class="mb-8 rounded-full w-24 h-24">
		<img alt="" src={avatar.url} />
	</div>
</div>

<div class="card text-center shadow-2xl">
	<figure class="px-10 pt-10">
		<img alt="" src="https://picsum.photos/id/1005/400/250" class="rounded-xl blur" />
	</figure>
	<div class="card-body">
		<h2 class="card-title">{name}</h2>
		<p>
			{bio}
		</p>
		<div class="justify-center card-actions">
			{#if polyworkUserBadges}
				<ul>
					{#each polyworkUserBadges as { name }}
						<li class="badge badge-primary mr-2 mb-2">{name}</li>
					{/each}
				</ul>
			{/if}
		</div>
	</div>
</div>

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

