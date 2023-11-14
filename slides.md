---
layout: cover
background: ./images/background.jpg
class: text-white
title: Graph & React Flow
---

# Graph & React Flow

<div  
  v-motion
  :delay="100"
  :initial="{ opacity: 0, y: 100 }"
  :enter="{ opacity: 1, y: 0, scale: 1 }"
  class="text-xl p-2">
  a new way of showing data
</div>

---

### Graph

[D3 Graph Theory](https://d3gt.com/index.html)

### React Flow

> A customizable React component for building node-based editors and interactive diagrams.

- [React Flow](https://reactflow.dev/)
- [Svelte Flow](https://svelteflow.dev/)
- from [xyflow](https://www.xyflow.com/about)

### Slidev

[slidev](https://sli.dev/)

- Vite
- Vue
- Tailwindcss

---

#### Concept

> Graph = Nodes + Edges

- Node
- Edge

```ts {4,5,6,7|9,10|all}  {lines:true}
import { Edge, Node } from 'reactflow';

// required --> id, position
const initialNodes: Node[] = [
  { id: '1', position: { x: 0, y: 0 }, data: { label: '1' } },
  { id: '2', position: { x: 0, y: 100 }, data: { label: '2' } },
];

const initialEdges: Edge = [{ id: 'e1-2', source: '1', target: '2' }];
```

---

#### Interactivity - use react hooks

- onNodesChange: callback for what to do when [nodes change](https://reactflow.dev/api-reference/react-flow#on-nodes-change).
- onEdgesChange: callback for what to do when [edges change](https://reactflow.dev/api-reference/react-flow#on-edges-change).

```ts
const [nodes, setNodes] = useState(initialNodes);
const [edges, setEdges] = useState(initialEdges);

const onNodesChange = useCallback(
  (changes: NodeChange[]) => setNodes((nds) => applyNodeChanges(changes, nds)),
  [setNodes]
);
const onEdgesChange = useCallback(
  (changes: EdgeChange[]) => setEdges((eds) => applyEdgeChanges(changes, eds)),
  [setEdges]
);
```

```ts {monaco-diff}
This line is removed on the right.
just some text
abcd
efgh
Some more text
~~~
just some text
abcz
zzzzefgh
Some more text.
This line is removed on the left.
```

---

#### Interactivity - use reactflow hooks

- onNodesChange
- onEdgesChange
- onConnect: callback for what to do when nodes are [connected](https://reactflow.dev/api-reference/react-flow#on-connect).

```jsx
import ReactFlow, { useNodesState, useEdgesState, addEdge } from 'reactflow';

export default function App() {
  const [nodes, setNodes, onNodesChange] = useNodesState(initialNodes);
  const [edges, setEdges, onEdgesChange] = useEdgesState(initialEdges);

  const onConnect = useCallback(
    (params) => setEdges((eds) => addEdge(params, eds)),
    [setEdges]
  );

  return (
    <div style={{ width: '100vw', height: '100vh' }}>
      <ReactFlow
        nodes={nodes}
        edges={edges}
        onNodesChange={onNodesChange}
        onEdgesChange={onEdgesChange}
        onConnect={onConnect}
      />
    </div>
  );
}
```

---

#### Built-in plugins

- The `<Background />` plugin implements some basic customizable background patterns.
- The `<MiniMap />` plugin displays a small version of the graph in the corner of the screen.
- The `<Controls />` plugin adds controls to zoom, center, and lock the viewport.
- The `<Panel />` plugin makes it easy to position content on top of the viewport.
- The `<NodeToolbar />` plugin lets you render a toolbar attached to a node.
- The `<NodeResizer />` plugin makes it easy to add resize functionality to your nodes.

---

#### Plugin examples

```jsx
  const nodeColor = (node: Node) => {
    switch (node.type) {
      case 'input':
        return '#6ede87';
      case 'output':
        return '#6865A5';
      default:
        return '#ff0072';
    }
  };

  const log = () => {
    console.log('nodes', nodes);
    console.log('edges', edges);
  };

...
      <ReactFlow
        ...
      >
        <MiniMap nodeColor={nodeColor} />
        <Controls />
        <Background variant={BackgroundVariant.Dots} gap={12} size={1} />
        <Panel position="top-left">
          <button onClick={log}>Log</button>
        </Panel>
      </ReactFlow>
```
