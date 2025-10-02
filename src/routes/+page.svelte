<script lang="ts">
  import timelineData from '$lib/data/timeline.json';
  import { fade, fly } from 'svelte/transition';

  type StudyStatus = {
    summarized: boolean;
    mapped: boolean;
    compared: boolean;
    linked: boolean;
  };

  type Study = {
    id: string;
    timestamp: string;
    title: string;
    status: StudyStatus;
  };

  // Filter studies by start/end of the given day in user's local timezone
  function fetchTimeline(date: string): Study[] {

    const start = new Date(date + "T00:00:00"); // Local midnight
    const end = new Date(start);
    end.setDate(end.getDate() + 1); // Midnight of next day

    return (timelineData as Study[])
      .filter(study => {
        const local = new Date(study.timestamp);
        return local >= start && local < end;
      })
      .sort(
        (a, b) =>
          new Date(a.timestamp).getTime() - new Date(b.timestamp).getTime()
      ); // Chronological
  }

  function formatDate(dateStr: string): string {
    const date = new Date(dateStr + "T00:00:00"); // Local midnight
    return date.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
  }

  function formatTime(timestamp: string): string {
    const date = new Date(timestamp);
    return date.toLocaleTimeString('en-US', {
      hour: 'numeric',
      minute: '2-digit',
      hour12: true
    });
  }

  function adjustDate(days: number) {
    const current = new Date(selectedDate + "T00:00:00"); // Local midnight
    current.setDate(current.getDate() + days);
    selectedDate = current.toLocaleDateString("en-CA"); // Always YYYY-MM-DD
    studies = fetchTimeline(selectedDate);
  }

  let selectedDate: string = "2025-10-02";
  let studies: Study[] = fetchTimeline(selectedDate);
</script>

<main class="p-6 min-h-screen bg-gradient-to-b from-[#ffffff] to-[#6ec5ff]">

  <!-- Header row with title and date selector -->
  <div class="flex justify-between items-end mb-6">
    <h1 class="text-6xl font-bold text-black">RadFlow</h1>
    <div class="flex items-center gap-2">
      <button class="frosty-button" on:click={() => adjustDate(-1)}>❮</button>
      <span class="frosty-glass px-4 py-1 text-black font-semibold text-lg w-24 text-center">
        {formatDate(selectedDate)}
      </span>
      <button class="frosty-button" on:click={() => adjustDate(1)}>❯</button>
    </div>
  </div>

  <!-- Container box -->
  <div class="p-6 glass min-h-[400px] flex gap-6">

    <!-- Timeline -->
    <div class="w-2/3 relative">
      <div class="absolute left-4 inset-y-0 w-px bg-black"></div> <!-- Line -->

      <div class="flex flex-col gap-5">
        {#each studies as study (study.id)}
          <div in:fade={{ duration: 400 }}> <!-- Outer wrapper handles fade -->
            <div class="relative" in:fly={{ y: -20, duration: 400 }}> <!-- Inner handles fly -->
              <div class="absolute left-4 top-6 w-2 h-2 rounded-full bg-black -translate-x-[calc(50%-0.5px)]"></div> <!-- Dot -->

              <!-- Card -->
              <div class="frosty-glass ml-12 p-4">
                <p class="font-medium">
                  <strong>{formatTime(study.timestamp)}</strong> – {study.title}
                </p>
                <div class="mt-2 flex flex-wrap gap-3 text-sm">
                  <span class="{study.status.summarized ? 'text-green-600 font-bold' : 'text-gray-400'}">
                    {study.status.summarized ? '✔' : '✘'} Summarized
                  </span>
                  <span class="{study.status.mapped ? 'text-green-600 font-bold' : 'text-gray-400'}">
                    {study.status.mapped ? '✔' : '✘'} Mapped
                  </span>
                  <span class="{study.status.compared ? 'text-green-600 font-bold' : 'text-gray-400'}">
                    {study.status.compared ? '✔' : '✘'} Compared
                  </span>
                  <span class="{study.status.linked ? 'text-green-600 font-bold' : 'text-gray-400'}">
                    {study.status.linked ? '✔' : '✘'} Linked
                  </span>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>

    <!-- Summary blocks stacked -->
    <div class="w-1/3 flex flex-col gap-6">
      <div class="frosty-glass p-4">
        <p><strong>Daily Digest</strong><br>Studies Today: {studies.length}</p>
      </div>
      <div class="frosty-glass p-4">
        <p><strong>Learning Loop</strong><br>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
      </div>
    </div>
  </div>
</main>

<style>
  main {
    font-family: sans-serif;
  }
</style>
