<script>
  import { format } from 'date-fns'

  export let activityDetails
</script>

<section>
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
        </div>
      </section>
    {/each}
  {/if}
</section>
