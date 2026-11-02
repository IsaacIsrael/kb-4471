<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { groupByCause, readAs, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { ticket, issues }: { ticket: number; issues: Record<string, string> } = $props();

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  const groups = $derived(groupByCause(merged, tenantOf));
  // Counted as the desk counts: by cause, each report once. The one-day-move reports stay under the
  // cause their tenant gives them and are footnoted, and they also get a ticket of their own.
  const moved = $derived(groups.reduce((n, g) => n + readAs(g, notes.oneDayMove), 0));
  const rows = $derived([
    ...groups.map((g) => ({ key: g.cause, count: g.reports.length })),
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
<p class="sum"><strong>{merged.length}</strong> reports merged into #{ticket}. The {moved} one-day-move reports are also counted under their cause.</p>
