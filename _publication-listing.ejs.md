```{=html}
<%
const sectionOrder = {
  "Publicações": 0,
  "Andamento": 1,
  "Outros": 2
};

const sortedItems = items
  .filter((item) => item.section !== "Andamento")
  .sort((a, b) => {
  const yearA = Number.isFinite(Number(a.year)) ? Number(a.year) : -Infinity;
  const yearB = Number.isFinite(Number(b.year)) ? Number(b.year) : -Infinity;

  if (yearB !== yearA) {
    return yearB - yearA;
  }

  const sectionA = sectionOrder[a.section] ?? 99;
  const sectionB = sectionOrder[b.section] ?? 99;
  if (sectionA !== sectionB) {
    return sectionA - sectionB;
  }

  return (a.title || "").localeCompare(b.title || "", "pt-BR");
});

let currentYear = null;
%>
<div class="pub-list list">
<% for (const item of sortedItems) { %>
  <% const yearLabel = item.year ? String(item.year) : "Sem ano"; %>
  <% if (yearLabel !== currentYear) { %>
    <% if (currentYear !== null) { %>
        </div>
      </section>
    <% } %>
    <% currentYear = yearLabel; %>
    <section class="pub-year-group" data-year-group="<%- yearLabel %>">
      <h2 class="pub-year-heading"><%= yearLabel %></h2>
      <div class="pub-year-group__items">
  <% } %>

  <article
    class="pub-item"
    <%= metadataAttrs(item) %>
    data-year="<%- item.year || '' %>"
    data-theme-values="<%- Array.isArray(item.categories) ? item.categories.join('|') : '' %>"
    data-subtheme-values="<%- Array.isArray(item.subthemes) ? item.subthemes.join('|') : '' %>"
  >
    <div class="pub-item__cover">
      <% if (item.image) { %>
        <img
          class="pub-item__cover-image listing-image"
          src="<%- item.image %>"
          alt="<%- item.image_alt || `Capa de ${item.title}` %>"
          loading="lazy"
        >
      <% } else { %>
        <div class="pub-item__cover-placeholder" aria-hidden="true"></div>
      <% } %>
    </div>

    <div class="pub-item__body">
      <% if ((Array.isArray(item.categories) && item.categories.length) || (Array.isArray(item.subthemes) && item.subthemes.length)) { %>
        <div class="pub-item__taxonomy">
          <ul class="pub-item__tag-list" aria-label="Temas e subtemas">
            <% if (Array.isArray(item.categories) && item.categories.length) { %>
              <% item.categories.forEach((category) => { %>
                <li class="pub-item__tag pub-item__tag--theme listing-categories"><%= category %></li>
              <% }) %>
            <% } %>

            <% if (Array.isArray(item.subthemes) && item.subthemes.length) { %>
              <% item.subthemes.forEach((subtheme) => { %>
                <li class="pub-item__tag pub-item__tag--subtheme"><%= subtheme %></li>
              <% }) %>
            <% } %>
          </ul>
        </div>
      <% } %>

      <h3 class="pub-item__title listing-title">
        <% if (item.href) { %>
          <a href="<%- item.href %>"><%= item.title %></a>
        <% } else { %>
          <span><%= item.title %></span>
        <% } %>
      </h3>

      <% if (item.authors) { %>
        <p class="pub-item__authors listing-authors"><%= item.authors %></p>
      <% } %>

      <% if (item.venue || item.note) { %>
        <p class="pub-item__citation">
          <% if (item.venue) { %>
            <span class="pub-item__venue listing-venue"><%= item.venue %></span>
          <% } %>
          <% if (item.note) { %>
            <span class="pub-item__note"><%= item.venue ? ' · ' : '' %><%= item.note %></span>
          <% } %>
        </p>
      <% } %>

      <% if (Array.isArray(item.resources) && item.resources.length) { %>
        <p class="pub-item__resources">
          Materiais:
          <% item.resources.forEach((resource, index) => { %>
            <% if (index > 0) { %>, <% } %>
            <a href="<%- resource.href %>"><%= resource.label %></a>
          <% }) %>
        </p>
      <% } %>

      <% if (Array.isArray(item.coverage) && item.coverage.length) { %>
        <p class="pub-item__coverage">
          Cobertura:
          <% item.coverage.forEach((coverageItem, index) => { %>
            <% if (index > 0) { %>, <% } %>
            <a href="<%- coverageItem.href %>"><%= coverageItem.label %></a>
          <% }) %>
        </p>
      <% } %>
    </div>
  </article>
<% } %>
<% if (currentYear !== null) { %>
      </div>
    </section>
<% } %>
</div>
```
