---
layout: default
title: Algorithms
permalink: /algorithms-list/
---

# Algorithms

<style>
.algo-toolbar {
  margin: 1rem 0;
  display: flex;
  gap: 0.5rem;
  align-items: center;
  flex-wrap: wrap;
}
.algo-input {
  padding: 0.45rem 0.6rem;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  min-width: 240px;
}
.tag-chip {
  display: inline-block;
  padding: 0.2rem 0.5rem;
  margin: 0.1rem 0.25rem 0.1rem 0;
  border-radius: 999px;
  font-size: 0.8rem;
  background: #e2e8f0;
  color: #1e293b;
  cursor: pointer;
}
.tag-chip:hover {
  background: #cbd5e1;
}
.algo-row {
  margin-bottom: 0.75rem;
}
.algo-hidden {
  display: none;
}
#tag-search-empty {
  margin-top: 1rem;
  font-style: italic;
}
</style>

<div class="algo-toolbar">
  <label for="tag-search">Search by tag:</label>
  <input id="tag-search" class="algo-input" type="text" placeholder="e.g. sorting, array" />
</div>

<div id="popular-tags"></div>

{% assign items = site.algorithms | sort: "title" %}
{% if items.size > 0 %}
<ul id="algorithms-list-root">
{% for item in items %}
  {% assign tag_csv = item.tags | join: "," | downcase %}
  <li class="algo-row" data-tags="{{ tag_csv }}">
    <a href="{{ item.url | relative_url }}">{{ item.title }}</a>{% if item.difficulty %} - {{ item.difficulty }}{% endif %}
    {% if item.tags and item.tags.size > 0 %}
      <div>
      {% for tag in item.tags %}
        <span class="tag-chip tag-clickable" data-tag="{{ tag | downcase }}">#{{ tag }}</span>
      {% endfor %}
      </div>
    {% endif %}
  </li>
{% endfor %}
</ul>
<div id="tag-search-empty" class="algo-hidden">No algorithms match this tag.</div>
{% else %}
No algorithm notes yet. Add one in `_algorithms/`.
{% endif %}

<script>
(function () {
  const input = document.getElementById("tag-search");
  const list = document.getElementById("algorithms-list-root");
  const empty = document.getElementById("tag-search-empty");
  const popularTags = document.getElementById("popular-tags");
  if (!input || !list || !empty || !popularTags) return;

  const rows = Array.from(list.querySelectorAll(".algo-row"));
  const tagCounts = new Map();

  rows.forEach((row) => {
    const tags = (row.dataset.tags || "").split(",").filter(Boolean);
    tags.forEach((tag) => tagCounts.set(tag, (tagCounts.get(tag) || 0) + 1));
  });

  const sortedTags = Array.from(tagCounts.entries()).sort((a, b) => b[1] - a[1] || a[0].localeCompare(b[0]));
  if (sortedTags.length > 0) {
    const label = document.createElement("p");
    label.textContent = "Tags:";
    popularTags.appendChild(label);
    sortedTags.forEach(([tag]) => {
      const chip = document.createElement("button");
      chip.type = "button";
      chip.className = "tag-chip";
      chip.textContent = "#" + tag;
      chip.addEventListener("click", () => {
        input.value = tag;
        filterRows();
      });
      popularTags.appendChild(chip);
    });
  }

  function filterRows() {
    const query = input.value.trim().toLowerCase();
    let visible = 0;

    rows.forEach((row) => {
      const tags = (row.dataset.tags || "").split(",").filter(Boolean);
      const match = query === "" || tags.some((t) => t.includes(query));
      row.classList.toggle("algo-hidden", !match);
      if (match) visible++;
    });

    empty.classList.toggle("algo-hidden", visible !== 0);
  }

  input.addEventListener("input", filterRows);

  list.addEventListener("click", (event) => {
    const target = event.target;
    if (!(target instanceof HTMLElement)) return;
    if (!target.classList.contains("tag-clickable")) return;
    const selectedTag = (target.dataset.tag || "").trim();
    if (!selectedTag) return;
    input.value = selectedTag;
    filterRows();
  });
})();
</script>
