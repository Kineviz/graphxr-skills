# Game of Thrones Graph Loader

<!-- {"dname":"parseCSV","pinCode":false,"codeMode":"js","hide":false} -->
```js
parseCSV = {
  return function parseCSV(text) {
    const lines = text.trim().split('\n');
    const headers = lines[0].split(',').map(h => h.trim());
    const rows = [];
    for (let i = 1; i < lines.length; i++) {
      const values = [];
      let current = '';
      let inQuotes = false;
      for (const char of lines[i]) {
        if (char === '"') { inQuotes = !inQuotes; }
        else if (char === ',' && !inQuotes) { values.push(current.trim()); current = ''; }
        else { current += char; }
      }
      values.push(current.trim());
      const row = {};
      headers.forEach((h, idx) => { row[h] = values[idx] || ''; });
      rows.push(row);
    }
    return rows;
  }
}
```

<!-- {"dname":"characters","pinCode":false,"codeMode":"js","hide":false} -->
```js
characters = {
  const resp = await gxr.files.get('Characters.csv');
  const text = await resp.text();
  return parseCSV(text);
}
```

<!-- {"dname":"episodes","pinCode":false,"codeMode":"js","hide":false} -->
```js
episodes = {
  const resp = await gxr.files.get('Episodes.csv');
  const text = await resp.text();
  const rows = parseCSV(text);
  return rows.map(r => ({
    ...r,
    seasonEpisode: 'S' + r.seasonNumber + 'E' + r.episodeNumber,
    seasonNumber: Number(r.seasonNumber),
    episodeNumber: Number(r.episodeNumber),
    millionViewers: Number(r.millionViewers),
  }));
}
```

<!-- {"dname":"lines","pinCode":false,"codeMode":"js","hide":false} -->
```js
lines = {
  const resp = await gxr.files.get('Lines.csv');
  const text = await resp.text();
  const rows = parseCSV(text);
  return rows.map(r => ({
    ...r,
    lineCount: Number(r.lineCount),
    wordCount: Number(r.wordCount),
  }));
}
```

<!-- {"dname":"writeGraph","pinCode":false,"codeMode":"js","hide":false} -->
```js
{
  return Button("Build Graph from CSVs", async () => {
    gxr.clear();

    // 1. Merge Character nodes
    await gxr.mergeNodes({
      category: "Character",
      keys: ["characterName"],
      data: characters.map(c => ({
        characterName: c.characterName,
        houseName: c.houseName,
        characterImage: c.characterImage,
        killedBy: c.killedBy,
        kills: Number(c.kills),
      })),
    });

    // 2. Merge Episode nodes
    await gxr.mergeNodes({
      category: "Episode",
      keys: ["seasonEpisode"],
      data: episodes.map(e => ({
        seasonEpisode: e.seasonEpisode,
        seasonNumber: e.seasonNumber,
        episodeNumber: e.episodeNumber,
        episodeTitle: e.episodeTitle,
        episodeAirDate: e.episodeAirDate,
        episodeDescription: e.episodeDescription,
        millionViewers: e.millionViewers,
      })),
    });

    // 3. Merge SPOKE_IN edges (Character -> Episode)
    await gxr.mergeRelationships({
      source: { category: "Character", keys: ["characterName"] },
      edge: { relationship: "SPOKE_IN", keys: ["characterName", "seasonEpisode"] },
      target: { category: "Episode", keys: ["seasonEpisode"] },
      data: lines.map(l => ({
        source: { characterName: l.speaker },
        edge: { characterName: l.speaker, seasonEpisode: l.seasonEpisode, lineCount: l.lineCount, wordCount: l.wordCount },
        target: { seasonEpisode: l.seasonEpisode },
      })),
    });

    // 4. Extract House nodes from Character.houseName
    await gxr.extract({
      sourceCategory: "Character",
      targetCategory: "House",
      props: [{ name: "houseName", isKey: true }],
      relationship: "BELONGS_TO",
    });

    // 5. Create KILLED_BY edges (Character -> Character)
    const killData = characters
      .filter(c => c.killedBy && c.killedBy.length > 0)
      .map(c => ({
        source: { characterName: c.characterName },
        edge: { victim: c.characterName, killer: c.killedBy },
        target: { characterName: c.killedBy },
      }));
    if (killData.length > 0) {
      await gxr.mergeRelationships({
        source: { category: "Character", keys: ["characterName"] },
        edge: { relationship: "KILLED_BY", keys: ["victim", "killer"] },
        target: { category: "Character", keys: ["characterName"] },
        data: killData,
      });
    }

    // 6. Style the graph
    gxr.setCategoryColor("Character", "#D4A017");
    gxr.setCategoryColor("Episode", "#4682B4");
    gxr.setCategoryColor("House", "#228B22");
    gxr.setCategoryIconByName("Character", "person");
    gxr.setCategoryIconByName("Episode", "play-circle");
    gxr.setCategoryIconByName("House", "home");
    gxr.sizeNodesByProperty({ category: "Character", property: "kills" });
    gxr.sizeNodesByProperty({ category: "Episode", property: "millionViewers" });

    // 7. Layout
    gxr.forceLayout();
    await gxr.sleep(3000);
    await gxr.flyToCenter();

    gxr.toast().success("Graph built! " + gxr.nodes().ids().length + " nodes, " + gxr.edges().ids().length + " edges.");
  });
}
```

## Graph Stats

<!-- {"dname":"stats","pinCode":false,"codeMode":"js","hide":false} -->
```js
{
  const charCount = graph.nodes({ category: "Character" }).ids().length;
  const epCount = graph.nodes({ category: "Episode" }).ids().length;
  const houseCount = graph.nodes({ category: "House" }).ids().length;
  const edgeCount = graph.edges().ids().length;
  return html`<div style="display:flex;gap:24px;flex-wrap:wrap;">
    <div><strong>${charCount}</strong> Characters</div>
    <div><strong>${epCount}</strong> Episodes</div>
    <div><strong>${houseCount}</strong> Houses</div>
    <div><strong>${edgeCount}</strong> Edges</div>
  </div>`;
}
```

## Top 10 Speakers

<!-- {"dname":"topSpeakers","pinCode":false,"codeMode":"js","hide":false} -->
```js
{
  const spokeEdges = graph.edges({ relationship: "SPOKE_IN" });
  const totals = {};
  spokeEdges.forEach(e => {
    const name = e.source.properties.characterName;
    const words = Number(e.properties.wordCount) || 0;
    totals[name] = (totals[name] || 0) + words;
  });
  const sorted = Object.entries(totals)
    .map(([name, words]) => ({ name, words }))
    .sort((a, b) => b.words - a.words)
    .slice(0, 10);
  return Plot.plot({
    marginLeft: 120,
    marks: [
      Plot.barX(sorted, { x: "words", y: "name", sort: { y: "-x" }, fill: "#D4A017" }),
      Plot.text(sorted, { x: "words", y: "name", text: d => d.words.toLocaleString(), dx: 4, textAnchor: "start" }),
    ],
    x: { label: "Total Words" },
    y: { label: null },
  });
}
```

## Viewership Over Time

<!-- {"dname":"viewership","pinCode":false,"codeMode":"js","hide":false} -->
```js
{
  const eps = graph.nodes({ category: "Episode" });
  const data = eps.map(n => ({
    seasonEpisode: n.properties.seasonEpisode,
    millionViewers: Number(n.properties.millionViewers) || 0,
    season: Number(n.properties.seasonNumber) || 0,
  }));
  data.sort((a, b) => {
    if (a.season !== b.season) return a.season - b.season;
    return String(a.seasonEpisode).localeCompare(String(b.seasonEpisode));
  });
  return Plot.plot({
    marginLeft: 50,
    marks: [
      Plot.line(data, { x: "seasonEpisode", y: "millionViewers", stroke: "#4682B4", strokeWidth: 2 }),
      Plot.dot(data, { x: "seasonEpisode", y: "millionViewers", fill: "#4682B4", r: 3 }),
    ],
    x: { label: "Episode", tickRotate: -45 },
    y: { label: "Million Viewers" },
  });
}
```

## Kill Leaderboard

<!-- {"dname":"KillTable","pinCode":false,"codeMode":"jsx","hide":false} -->
```jsx
function KillTable() {
  const { Table } = Antd;
  const chars = graph.nodes({ category: "Character" })
    .map(n => n.properties)
    .filter(p => Number(p.kills) > 0)
    .sort((a, b) => Number(b.kills) - Number(a.kills));
  const columns = [
    { title: "Character", dataIndex: "characterName", key: "characterName" },
    { title: "Kills", dataIndex: "kills", key: "kills", sorter: (a, b) => a.kills - b.kills },
    { title: "Killed By", dataIndex: "killedBy", key: "killedBy" },
  ];
  return <Table dataSource={chars} columns={columns} rowKey="characterName" size="small" pagination={{ pageSize: 10 }} />;
}
```

<!-- {"dname":"killTableRender","pinCode":false,"codeMode":"jsx","hide":false} -->
```jsx
react(<KillTable />)
```

## House Filter

<!-- {"dname":"houseFilter","pinCode":false,"codeMode":"js","hide":false} -->
```js
viewof selectedHouse = {
  const houses = [...new Set(
    graph.nodes({ category: "Character" })
      .map(n => n.properties.houseName)
      .filter(h => h && h.length > 0)
  )].sort();
  return Inputs.select(["(all)", ...houses], { label: "Select House" });
}
```

<!-- {"dname":"highlightHouse","pinCode":false,"codeMode":"js","hide":false} -->
```js
{
  gxr.deselectNodes();
  if (selectedHouse && selectedHouse !== "(all)") {
    graph.nodes({ category: "Character", properties: { houseName: selectedHouse } }).select();
  }
}
```