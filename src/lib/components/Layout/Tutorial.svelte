<script lang="ts">
  import { Button } from '$/components/ui/button';
  import RocketIcon from '~icons/material-symbols/rocket-launch';
  import CategoryIcon from '~icons/material-symbols/category';
  import LightbulbIcon from '~icons/material-symbols/lightbulb';
  import { getSampleDiagrams } from '$/util/mermaid';
  import { updateCode } from '$/util/state';
  import { toast } from 'svelte-sonner';

  const diagrams = [
    { id: 'flowchart', label: 'Flowchart', icon: '⑂' },
    { id: 'sequence', label: 'Sequence', icon: '⇄' },
    { id: 'class', label: 'Class', icon: '◆' },
    { id: 'state', label: 'State', icon: '◎' },
    { id: 'er', label: 'ER Diagram', icon: '≡' },
    { id: 'gantt', label: 'Gantt', icon: '≣' },
    { id: 'mindmap', label: 'Mindmap', icon: '❆' },
    { id: 'gitGraph', label: 'Git Graph', icon: '' },
    { id: 'timeline', label: 'Timeline', icon: '◷' },
    { id: 'pie', label: 'Pie Chart', icon: '◔' }
  ];

  let activeTab = 'start';

  function loadExample(type: string) {
    const samples = getSampleDiagrams();
    const sample = Object.entries(samples).find(([key]) => key.toLowerCase().includes(type));
    if (sample) {
      updateCode(sample[1], { resetPanZoom: true, updateDiagram: true });
      toast.success(`Loaded ${type} example`);
    } else {
      toast.error(`No example found for ${type}`);
    }
  }
</script>

<div class="flex h-full flex-col overflow-hidden bg-background">
  <div class="flex items-center gap-2 border-b p-4">
    <RocketIcon class="size-5 text-primary" />
    <h2 class="font-bold tracking-tight">Learning Center</h2>
  </div>

  <div class="flex flex-1 flex-col overflow-hidden">
    <div class="border-b px-4 pt-2">
      <div class="grid w-full grid-cols-3 gap-1">
        <button
          class="border-b-2 px-3 py-2 text-sm font-medium transition-colors"
          class:border-primary={activeTab === 'start'}
          class:text-primary={activeTab === 'start'}
          class:border-transparent={activeTab !== 'start'}
          class:text-muted-foreground={activeTab !== 'start'}
          onclick={() => (activeTab = 'start')}>
          Start
        </button>
        <button
          class="border-b-2 px-3 py-2 text-sm font-medium transition-colors"
          class:border-primary={activeTab === 'types'}
          class:text-primary={activeTab === 'types'}
          class:border-transparent={activeTab !== 'types'}
          class:text-muted-foreground={activeTab !== 'types'}
          onclick={() => (activeTab = 'types')}>
          Types
        </button>
        <button
          class="border-b-2 px-3 py-2 text-sm font-medium transition-colors"
          class:border-primary={activeTab === 'tips'}
          class:text-primary={activeTab === 'tips'}
          class:border-transparent={activeTab !== 'tips'}
          class:text-muted-foreground={activeTab !== 'tips'}
          onclick={() => (activeTab = 'tips')}>
          Tips
        </button>
      </div>
    </div>

    <div class="flex-1 overflow-y-auto p-4">
      {#if activeTab === 'start'}
        <div class="space-y-6">
          <section class="space-y-3">
            <h3 class="flex items-center gap-2 text-lg font-semibold">
              <span class="rounded bg-primary/10 p-1 text-primary">1</span>
              The Editor
            </h3>
            <p class="text-sm leading-relaxed text-muted-foreground">
              Write Mermaid code in the <strong>Code</strong> tab. See it render instantly. Use the
              <strong>Config</strong> tab to tweak themes and settings.
            </p>
          </section>

          <section class="space-y-3">
            <h3 class="flex items-center gap-2 text-lg font-semibold">
              <span class="rounded bg-primary/10 p-1 text-primary">2</span>
              File Management
            </h3>
            <p class="text-sm leading-relaxed text-muted-foreground">
              Open local folders to manage projects. Look for the <span
                class="font-bold text-primary">●</span> dot indicating unsaved changes. Autosave runs
              every 60s.
            </p>
          </section>

          <Button
            variant="outline"
            class="w-full gap-2"
            href="https://mermaid.js.org/intro/"
            target="_blank">
            <LightbulbIcon />
            Read Official Docs
          </Button>
        </div>
      {/if}

      {#if activeTab === 'types'}
        <div class="grid grid-cols-2 gap-2">
          {#each diagrams as diag}
            <button
              class="flex flex-col items-center justify-center gap-2 rounded-lg border bg-card p-4 text-center transition-all hover:border-primary/50 hover:bg-muted/50"
              onclick={() => loadExample(diag.id)}>
              <span class="text-2xl opacity-70">{diag.icon}</span>
              <span class="text-xs font-medium">{diag.label}</span>
            </button>
          {/each}
        </div>
      {/if}

      {#if activeTab === 'tips'}
        <div class="space-y-4">
          <div class="rounded-lg border bg-muted/20 p-3">
            <h4 class="mb-1 text-sm font-semibold text-accent">Sidebar Navigation</h4>
            <p class="text-xs text-muted-foreground">
              Use the activity bar on the left to switch between Explorer, History, and Templates.
            </p>
          </div>
          <div class="rounded-lg border bg-muted/20 p-3">
            <h4 class="mb-1 text-sm font-semibold text-accent">AI Assistant</h4>
            <p class="text-xs text-muted-foreground">
              Click the "✨ AI" button in the header to generate diagrams from text descriptions.
            </p>
          </div>
          <div class="rounded-lg border bg-muted/20 p-3">
            <h4 class="mb-1 text-sm font-semibold text-accent">Exporting</h4>
            <p class="text-xs text-muted-foreground">
              Use the Export tab to save your diagram as PNG, SVG, or Markdown.
            </p>
          </div>
        </div>
      {/if}
    </div>
  </div>
</div>
