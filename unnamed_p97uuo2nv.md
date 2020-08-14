---
tags:
    - crossnote/beta3
created: 2020-07-06T09:37:46.977Z
modified: 2020-08-08T13:35:11.988Z
---
# Crossnote Network Graph preparation

#crossnote/beta3 

I think it is good to use D3
* https://zoomcharts.com/developers/en/net-chart/examples/gallery/network-drag-onto-nodes.html
* https://github.com/tchayen/markdown-links/blob/master/static/webview.html
* https://www.d3-graph-gallery.com/graph/network_basic.html
* https://bl.ocks.org/mapio/53fed7d84cd1812d6a6639ed7aa83868  (**d3 v5**)


Also need to add forces.
I am thinking should it become directional graph or undirectional graph

```javascript
var data = {
    "nodes": [
        { "id": "n1", "loaded": true, "style": { "label": "Node1" } },
        { "id": "n2", "loaded": true, "style": { "label": "Node2" } },
        { "id": "n3", "loaded": true, "style": { "label": "Node3" } }
    ],
    "links": [
        { "id": "l1", "from": "n1", "to": "n2" },
        { "id": "l2", "from": "n2", "to": "n3" },
        { "id": "l3", "from": "n3", "to": "n1" }
    ]
};
```
