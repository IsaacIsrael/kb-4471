<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { groupByCause, readAs, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { ticket, issues }: { ticket: number; issues: Record<string, string> } = $props();

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  const groups = $derived(groupByCause(merged, tenantOf));
  // The desk's one-day-move reports are filed under the cause their tenant gives them; they are a third problem.
  const moved = $derived(groups.reduce((n, g) => n + readAs(g, notes.oneDayMove), 0));
  const rows = $derived([
    ...groups.map((g) => ({ key: g.cause, count: g.reports.length - readAs(g, notes.oneDayMove) })),
    { key: 'one-day-move', count: moved },
  ]);
</script>

<table>
  <thead><tr><th>problem</th><th>reports</th><th>ticket</th></tr></thead>
  <tbody>
    {#each rows as r (r.key)}
      <tr><td><code>{r.key}</code></td><td>{r.count}</td><td><a href={issues[r.key]}>{issues[r.key]?.split('/').pop() ? `#${issues[r.key].split('/').pop()}` : ''}</a></td></tr>
    {/each}
  </tbody>
</table>
<p class="sum"><strong>{rows.reduce((n, r) => n + r.count, 0)}</strong> reports in all, from {merged.length} merged into #{ticket}.</p>
