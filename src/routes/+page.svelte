<script lang="ts">
  import {
    createVirtualizedTree,
    type TreeItem,
    VirtualizedTree,
  } from "svelte-virtualized-tree";
  type CustomTreeItem = {
    displayName: string;
  };
  let totalNodes = 0;
  function generateTreeData(
    level: number,
    parentId: number | null,
    count: number
  ): TreeItem<CustomTreeItem>[] {
    const items: TreeItem<CustomTreeItem>[] = [];

    for (let i = 0; i < count; i++) {
      const id = Math.floor(Math.random() * 10000);
      const item: TreeItem<CustomTreeItem> = {
        id,
        displayName: `Node ${id}`,
        icon: "plus",
        level,
        parentId,
        children:
          level < 4
            ? generateTreeData(level + 1, id, Math.floor(Math.random() * 3))
            : [],
      };
      totalNodes++;
      items.push(item);
    }
    return items;
  }
  const data = generateTreeData(0, null, 1000);
  const virtualizedTree = createVirtualizedTree({
    data,
    accessorKey: "displayName",
  });
</script>

<div>
  <h1>Virtualized Tree</h1>
  <p>
    This is a virtualized tree component that renders a large number of nodes
    efficiently.
  </p>
  <p>
    Total nodes: {totalNodes}
  </p>
  <div style="height: 400px; overflow-y: auto;">
    <VirtualizedTree {virtualizedTree} />
  </div>
</div>
