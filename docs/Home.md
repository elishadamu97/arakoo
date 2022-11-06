---
layout: default

---
<iframe width="708" height="423" src="https://www.youtube.com/embed/GTid9iwWX0Y" title="Replicache / Repliear Demo - 20220503.4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


<br>

![linear-filter](../image/linear-filter.gif)
<br>
<br>
![Memory View Architecture](../image/memory-view-architecture.webp)

<br>
```tsx
const memView = {
  filtered: [],
};

// every watch event triggers a reducer
rep.experimentalWatch(
  (diff) => {
    dispatch({
      type: "diff",
      diff,
    });
  },
  { prefix: ISSUE_KEY_PREFIX, initialValuesInFirstDiff: true }
);

// example of handling diff operation "change" in a reducer
const newFiltered = [...state.filtered];

for (const diffOp of diff) {
  // del and change cases
  if (diffOp.oldValue) {
    const oldIssue = diffOp.oldValue;
    if (state.filter(oldIssue)) {
      const index = sortedIndexBy(newFiltered, oldIssue, state.order);
      if (newFiltered[index]?.id === oldIssue.id) {
        newFiltered.splice(index, 1);
      }
    }
  }

  // change and add cases
  if (diffOp.newValue) {
    const newIssue = diffOp.newValue;
    if (state.filter(newIssue)) {
      newFiltered.splice(
        sortedIndexBy(newFiltered, newIssue, state.order),
        0,
        newIssue
      );
    }
  }
}

return {
  ...state,
  filtered: newFiltered
};
```