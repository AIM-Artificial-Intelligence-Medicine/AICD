---
layout: default
title: Community Case Reports
nav_order: 8
---

# Community-Submitted Case Reports

Each case is formatted using the DSA-1 clinical taxonomy, and includes structured fields such as symptoms, severity, mechanisms, and interventions.

---
<a class="btn" href="https://github.com/MAI-Medicine-of-Artificial-Intelligence/DSA/issues/new?template=case_report.yml">
  🩺 Report a Case
</a>
<!-- 👇 これが抜けていた！ -->
{% assign sorted_cases = site.cases | sort: "title" %}
<div style="margin-top: 1em; display: flex; gap: 1em; flex-wrap: wrap;">
  <label>
    <strong>Filter by Chapter:</strong>
    <select id="filter-chapter">
      <option value="">All</option>
      {% for letter in "A,B,C,D,E,F,G,H,I" | split: "," %}
        <option value="{{ letter }}">Chapter {{ letter }}</option>
      {% endfor %}
    </select>
  </label>

  <label>
    <strong>Filter by Severity:</strong>
    <select id="filter-severity">
      <option value="">All</option>
      <option value="4">4 – Severe</option>
      <option value="3">3 – Moderate</option>
      <option value="2">2 – Mild</option>
      <option value="1">1 – No harm</option>
    </select>
  </label>
</div>

---
{% for case in sorted_cases %}
  {% assign disorder_code = case.disorder | strip | split: " " | first %}
  {% assign chapter_letter = disorder_code | slice: 0, 1 | upcase %}


<article 
  class="case-entry"
  style="margin-bottom: 3em; padding: 1.5em; border-left: 4px solid #ccc; background: #f9f9f9;"
  data-chapter="{{ chapter_letter }}"
  data-severity="{{ case.severity | slice: 0, 1 }}">


  {% if case.disorder %}
    <p><strong>Disorder code (DSA-1):</strong> {{ case.disorder }}</p>
  {% endif %}
  {% if case.model %}
    <p><strong>Model / Version:</strong> {{ case.model }}</p>
  {% endif %}
  {% if case.symptoms %}
    <p><strong>Symptom(s) observed:</strong><br>{{ case.symptoms | markdownify }}</p>
  {% endif %}

  <details>
    <summary style="cursor: pointer; font-weight: bold; margin-top: 1em;"> Show full case report</summary>
    <div style="margin-top: 1em;">

      {% if case.repro %}
        <p><strong>Failure description & reproduction steps:</strong><br>{{ case.repro | markdownify }}</p>
      {% endif %}
      {% if case.severity %}
        <p><strong>Severity (DSA-1):</strong> {{ case.severity }}</p>
      {% endif %}
      {% if case.intervention %}
        <p><strong>Intervention or treatment:</strong><br>{{ case.intervention | markdownify }}</p>
      {% endif %}
      {% if case.outcome %}
        <p><strong>Outcome / Follow-up:</strong><br>{{ case.outcome | markdownify }}</p>
      {% endif %}
      {% if case.evidence and case.evidence != "_No response_" %}
        <p><strong>Evidence (e.g., URLs, logs):</strong><br>{{ case.evidence | markdownify }}</p>
      {% endif %}
      {% if case.mechanism %}
        <p><strong>Presumed underlying mechanism:</strong><br>{{ case.mechanism | markdownify }}</p>
      {% endif %}
      {% if case.detectability %}
        <p><strong>Detectability of failure:</strong> {{ case.detectability }}</p>
      {% endif %}
      {% if case.occurrence %}
        <p><strong>Estimated frequency / prevalence:</strong> {{ case.occurrence }}</p>
      {% endif %}
      {% if case.confidence %}
        <p><strong>Diagnostic confidence:</strong> {{ case.confidence }}</p>
      {% endif %}
      {% if case.algorithm %}
        <p><strong>Diagnostic pathway (if applicable):</strong> {{ case.algorithm }}</p>
      {% endif %}

    </div>
  </details>
</article>
{% endfor %}

<script>
  const chapterFilter = document.getElementById("filter-chapter");
  const severityFilter = document.getElementById("filter-severity");
  const entries = document.querySelectorAll(".case-entry");

  function applyFilters() {
    const selectedChapter = chapterFilter.value;
    const selectedSeverity = severityFilter.value;

    entries.forEach(entry => {
      const entryChapter = entry.dataset.chapter;
      const entrySeverity = entry.dataset.severity;

      const matchChapter = !selectedChapter || entryChapter === selectedChapter;
      const matchSeverity = !selectedSeverity || entrySeverity === selectedSeverity;

      entry.style.display = (matchChapter && matchSeverity) ? "block" : "none";
    });
  }

  chapterFilter.addEventListener("change", applyFilters);
  severityFilter.addEventListener("change", applyFilters);
</script>
